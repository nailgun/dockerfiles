# deis-router-proxy

Used as a workeround to have [deis](https://deis.com/workflow/) router to listen on ports 80 and 443 until [this issue](https://github.com/kubernetes/kubernetes/issues/23920) is resolved.

## Usage

Inject the proxy to your Deis cluster using the following spec:


```
---
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: deis-router-proxy
  namespace: deis
  labels:
    run: deis-router-proxy
spec:
  replicas: 1
  selector:
    matchLabels:
      run: deis-router-proxy
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: deis-router-proxy
    spec:
      containers:
      - name: deis-router-proxy
        image: nailgun/deis-router-proxy
        ports:
        - hostPort: 443
          containerPort: 443
          protocol: TCP
        - hostPort: 80
          containerPort: 80
          protocol: TCP
        - hostPort: 2222
          containerPort: 2222
          protocol: TCP
        env:
        - name: ROUTER_HOST
          value: 10.111.26.113
        resources: {}
        terminationMessagePath: /dev/termination-log
        imagePullPolicy: Always
      restartPolicy: Always
      terminationGracePeriodSeconds: 30
      dnsPolicy: ClusterFirst
      hostNetwork: true
      securityContext: {}
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
```

Change environment variable `ROUTER_HOST` to cluster ip of deis-router. This is required until there is [another issue](https://github.com/kubernetes/kubernetes/issues/17406) which prevents cluster DNS from working in pods with `hostNetwork`. By default `ROUTER_HOST` is `deis-router.deis.svc.cluster.local`.
