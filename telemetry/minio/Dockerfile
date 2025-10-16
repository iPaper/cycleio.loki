FROM minio/minio:latest

# Create startup script: Alloy runs in background, flog runs in foreground
RUN echo '' > /entrypoint.sh && \
    echo $'#!/bin/bash\n\
setupBuckets () {\n\
sleep 30; # Wait for MinIO to be fully up and running\n\
# Setup MinIO CLI alias\n\
mc alias set local http://localhost:9000 loki supersecret;\n\
# Create MinIO buckets\n\
mc mb local/mimir-blocks;\n\
mc mb local/mimir-ruler;\n\
mc mb local/loki-data;\n\
}\n\
setupBuckets &\n\
# Run MinIO as main process (foreground)\n\
/usr/bin/minio server /data\n\
' >> /entrypoint.sh

RUN chmod +x /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]