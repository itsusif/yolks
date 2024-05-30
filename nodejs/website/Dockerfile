FROM --platform=$TARGETOS/$TARGETARCH node:18-bookworm-slim

LABEL author="Michael Parker" maintainer="parker@pterodactyl.io"

RUN apt update \
    && apt -y install ffmpeg iproute2 git sqlite3 libsqlite3-dev python3 python3-dev ca-certificates dnsutils tzdata zip tar curl build-essential libtool iputils-ping libnss3 tini nginx \
    && useradd -m -d /home/container container

RUN npm install npm@9.8.1 typescript ts-node @types/node --location=global

# Nginx Configuration
COPY ./../nginx.conf /etc/nginx/nginx.conf

# Ensure the /tmp directory exists and is writable
RUN mkdir -p /tmp/client_body_temp /tmp/proxy_temp /tmp/fastcgi_temp /tmp/uwsgi_temp /tmp/scgi_temp \
    && chmod -R 777 /tmp
    && chmod -R 777 /run

USER container
ENV USER=container HOME=/home/container
WORKDIR /home/

STOPSIGNAL SIGINT

COPY --chown=container:container ./../v3.sh /v3.sh
RUN chmod +x /v3.sh

ENTRYPOINT ["/usr/bin/tini", "-g", "--"]
CMD ["sh", "-c", "nginx && /v3.sh"]
