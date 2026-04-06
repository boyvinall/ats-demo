FROM trafficserver/trafficserver:10.1.0

# OCI image labels for metadata
LABEL org.opencontainers.image.title="Apache Traffic Server" \
      org.opencontainers.image.description="ATS proxy cache with security hardening" \
      org.opencontainers.image.version="10.1.0" \
      org.opencontainers.image.vendor="ATS Cluster Example"

# Install curl for health checks and create cache directory with proper ownership
# Combine operations to reduce layers and ensure proper permissions before USER switch
RUN apt-get update && \
    apt-get install -y curl && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    mkdir -p /cache/trafficserver && \
    chown -R nobody:nogroup /cache/trafficserver && \
    chmod 755 /cache/trafficserver

# Copy all configuration files with proper ownership (single layer for efficiency)
COPY --chown=nobody:nogroup config/ /opt/etc/trafficserver/

# Expose ATS port
EXPOSE 8080

# Health check - verify ATS is responding
HEALTHCHECK --interval=10s --timeout=3s --start-period=15s --retries=5 \
    CMD curl -f http://localhost:8080/_stats || exit 1

# Switch to unprivileged user for security (run as nobody instead of root)
USER nobody

# Start Traffic Server (use default entrypoint from base image)
CMD ["traffic_server"]
