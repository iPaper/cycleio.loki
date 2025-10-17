# Build stage with shell and package manager
FROM alpine:latest AS builder
RUN apk add --no-cache busybox curl

FROM grafana/mimir:latest
# Copy shell and utilities from builder stage
COPY --from=builder /bin/sh /bin/sh
COPY --from=builder /bin/busybox /bin/busybox
COPY --from=builder /usr/bin/curl /usr/bin/curl

COPY ./mimir-config.yaml /etc/mimir/config.yaml
