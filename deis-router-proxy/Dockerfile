FROM haproxy:alpine

ENV ROUTER_HOST deis-router.deis.svc.cluster.local
ENV ROUTER_HTTP_PORT 80
ENV ROUTER_HTTPS_PORT 443
ENV ROUTER_BUILDER_PORT 2222
ENV ROUTER_HEALTHZ_PORT 9090

COPY haproxy.cfg /usr/local/etc/haproxy/haproxy.cfg

EXPOSE 80
EXPOSE 443
