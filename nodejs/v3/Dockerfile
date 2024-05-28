FROM        --platform=$TARGETOS/$TARGETARCH node:18-bookworm-slim

LABEL       author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN         apt update \
            && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini \
            && useradd -m -d /home/container container

RUN         npm install npm@9.8.1 typescript ts-node @types/node --location=global

# Create test directory
RUN         mkdir -p /home/container/test

USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container/test


STOPSIGNAL SIGINT

# Use the same relative path for consistency
COPY        --chown=container:container ./../v3.sh /v3.sh
RUN         chmod +x /v3.sh

ENTRYPOINT  ["/usr/bin/tini", "-g", "--"]
CMD         ["/v3.sh"]