FROM --platform=$TARGETOS/$TARGETARCH node:18-bookworm-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

# Install necessary packages
RUN         apt update \
            && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
            && useradd -m -d /home/container container

# Install npm packages globally
RUN         npm install npm@9.8.1 typescript ts-node @types/node --location=global

# Switch to non-root user
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

# Temporarily switch to root to create the directory and set permissions
USER root
RUN mkdir -p /home/container/config && chmod 700 /home/container/config
USER container

# Set permissions for the /home/container directory
RUN mkdir -p /home/container/bot && chmod 500 /home/container/bot

STOPSIGNAL SIGINT

# Copy entrypoint script and set permissions
COPY --chown=container:container ./entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Set entrypoint and default command
ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["/entrypoint.sh"]