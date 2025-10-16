FROM minio/minio:latest

# Create startup script: Alloy runs in background, flog runs in foreground
RUN echo '#!/bin/bash' > /entrypoint.sh && \
    echo 'setupBuckets () {
    # Setup MinIO CLI alias
    mc alias set local http://localhost:9000 loki supersecret;

    # Create MinIO buckets
    mc mb local/mimir-blocks;
    mc mb local/mimir-ruler;
    mc mb local/loki-data;
}' >> /entrypoint.sh && \
    echo 'setupBuckets &' >> /entrypoint.sh && \
    echo '# Run MinIO as main process (foreground)' >> /entrypoint.sh && \
    echo '/usr/bin/minio server /data&' >> /entrypoint.sh && \
    chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]