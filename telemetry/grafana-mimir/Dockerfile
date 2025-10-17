FROM grafana/mimir:latest

RUN apk add --no-cache busybox

COPY ./mimir-config.yaml /etc/mimir/config.yaml
