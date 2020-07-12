FROM resin/rpi-raspbian:jessie

# install deps
RUN apt-get update && \
    apt-get install -y curl && \
    rm -rf /var/lib/apt/lists/*

# download caddy
RUN mkdir -p /srv/wordpress && \
    curl -sL  https://github.com/caddyserver/caddy/releases/download/v2.1.1/caddy_2.1.1_linux_armv7.tar.gz | \
    tar xz -C /tmp/ && mv /tmp/caddy /usr/local/bin/caddy && \
    chmod +x /usr/local/bin/caddy

EXPOSE 80 443

WORKDIR /srv

CMD caddy run