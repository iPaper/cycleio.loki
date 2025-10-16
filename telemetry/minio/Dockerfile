FROM minio/minio:latest

# Create startup script: Alloy runs in background, flog runs in foreground
RUN echo '#!/bin/bash' > /entrypoint.sh && \
    echo '# Setup MinIO CLI alias' >> /entrypoint.sh && \
    echo 'mc alias set local http://localhost:9000 loki supersecret' >> /entrypoint.sh && \
    echo '' >> /entrypoint.sh && \
    echo '# Create MinIO buckets' >> /entrypoint.sh && \
    echo 'mc mb local/mimir-blocks' >> /entrypoint.sh && \
    echo 'mc mb local/mimir-ruler' >> /entrypoint.sh && \
    echo 'mc mb local/loki-data' >> /entrypoint.sh && \
    echo '' >> /entrypoint.sh && \
    echo '# Run MinIO as main process (foreground)' >> /entrypoint.sh && \
    echo '/usr/bin/minio server /data' >> /entrypoint.sh && \
    chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]