FROM trafficserver/trafficserver:10.1.0

# Install curl for health checks
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Copy configuration files to correct path for official image
COPY records.yaml /opt/etc/trafficserver/records.yaml
COPY remap.config /opt/etc/trafficserver/remap.config
COPY storage.config /opt/etc/trafficserver/storage.config
COPY plugin.config /opt/etc/trafficserver/plugin.config
COPY logging.yaml /opt/etc/trafficserver/logging.yaml

# Create cache directory
RUN mkdir -p /cache/trafficserver && chmod 755 /cache/trafficserver

# Expose ATS port
EXPOSE 8080

# Health check - verify ATS is responding
HEALTHCHECK --interval=10s --timeout=3s --start-period=15s --retries=5 \
    CMD curl -f http://localhost:8080/_stats || exit 1

# Start Traffic Server (use default entrypoint from base image)
CMD ["traffic_server"]
