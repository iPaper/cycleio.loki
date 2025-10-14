FROM grafana/alloy:latest

# Install flog log generator
RUN apk add --no-cache curl && \
    curl -L https://github.com/mingrammer/flog/releases/download/v0.4.3/flog_0.4.3_linux_amd64.tar.gz -o /tmp/flog.tar.gz && \
    tar -xzf /tmp/flog.tar.gz -C /usr/local/bin && \
    chmod +x /usr/local/bin/flog && \
    rm /tmp/flog.tar.gz

COPY ./config.alloy /etc/alloy/config.alloy

# Create a startup script that runs both flog and alloy
RUN echo '#!/bin/sh' > /entrypoint.sh && \
    echo 'touch /tmp/flog.log' >> /entrypoint.sh && \
    echo 'flog -f apache_combined -l -d 1s > /tmp/flog.log 2>&1 &' >> /entrypoint.sh && \
    echo 'exec /bin/alloy run --server.http.listen-addr=0.0.0.0:12345 --storage.path=/var/lib/alloy/data /etc/alloy/config.alloy "$@"' >> /entrypoint.sh && \
    chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
