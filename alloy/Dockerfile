FROM mcr.microsoft.com/dotnet/aspnet:9.0

# Install flog (main application)
RUN apt update && apt install -y curl ca-certificates && \
    ARCH=$(dpkg --print-architecture) && \
    if [ "$ARCH" = "amd64" ]; then FLOG_ARCH="amd64"; \
    elif [ "$ARCH" = "arm64" ]; then FLOG_ARCH="arm64"; \
    else echo "Unsupported architecture: $ARCH" && exit 1; fi && \
    curl -L https://github.com/mingrammer/flog/releases/download/v0.4.3/flog_0.4.3_linux_${FLOG_ARCH}.tar.gz -o /tmp/flog.tar.gz && \
    tar -xzf /tmp/flog.tar.gz -C /usr/local/bin && \
    chmod +x /usr/local/bin/flog && \
    rm /tmp/flog.tar.gz

# Install Grafana Alloy (observability sidecar)
RUN ARCH=$(dpkg --print-architecture) && \
    curl -L -o /tmp/alloy.deb "https://github.com/grafana/alloy/releases/download/v1.11.2/alloy-1.11.2-1.${ARCH}.deb" && \
    dpkg -i /tmp/alloy.deb && \
    rm /tmp/alloy.deb && \
    apt clean && rm -rf /var/lib/apt/lists/*

# Copy Alloy configuration
COPY ./config.alloy /etc/alloy/config.alloy

# Create startup script: Alloy runs in background, flog runs in foreground
RUN echo '#!/bin/bash' > /entrypoint.sh && \
    echo 'touch /tmp/flog.log' >> /entrypoint.sh && \
    echo '# Start Alloy as background service' >> /entrypoint.sh && \
    echo '/usr/bin/alloy run --server.http.listen-addr=0.0.0.0:12345 --storage.path=/var/lib/alloy/data /etc/alloy/config.alloy &' >> /entrypoint.sh && \
    echo 'ALLOY_PID=$!' >> /entrypoint.sh && \
    echo 'echo "Started Alloy with PID $ALLOY_PID"' >> /entrypoint.sh && \
    echo '' >> /entrypoint.sh && \
    echo '# Run flog as main process (foreground)' >> /entrypoint.sh && \
    echo 'exec flog -f apache_combined -l -d 1s > /tmp/flog.log 2>&1' >> /entrypoint.sh && \
    chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]
