FROM haproxy:alpine

ENV ROUTER_HOST deis-router.deis.svc.cluster.local
ENV ROUTER_HTTP_PORT 80
ENV ROUTER_HTTPS_PORT 443

COPY haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg

EXPOSE 80
EXPOSE 443
