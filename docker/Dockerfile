FROM debian:stable-slim

# Install OpenArena server and dependencies
RUN apt-get update && \
    apt-get install -y openarena-server && \
    rm -rf /var/lib/apt/lists/*

# Set working directory
WORKDIR /srv/openarena

# Copy your custom server.cfg and any additional maps (optional)
# COPY server.cfg /srv/openarena/baseoa/server.cfg
# COPY maps/*.pk3 /srv/openarena/baseoa/

# Expose the default OpenArena server port
EXPOSE 27960/udp

# Start the server with the desired configuration
CMD ["openarena-server", "+set", "fs_homepath", "/srv/openarena", "+exec", "baseoa/server.cfg"]
