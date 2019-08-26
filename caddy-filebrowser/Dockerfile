FROM alpine

RUN apk add --no-cache bash curl ca-certificates

# caddy
RUN curl https://getcaddy.com | bash -s personal "${PLUGINS}"

# Download and Install filebrowser
RUN mkdir /usr/local/filebrowser
ADD https://github.com/filebrowser/filebrowser/releases/download/v2.0.12/linux-amd64-filebrowser.tar.gz /usr/local/filebrowser/
RUN tar xzf /usr/local/filebrowser/linux-amd64-filebrowser.tar.gz -C /usr/local/filebrowser/ \
	&& rm /usr/local/filebrowser/linux-amd64-filebrowser.tar.gz

# copy caddy binary and ca certs
COPY /usr/local/bin/caddy /usr/bin/caddy
COPY /usr/local/filebrowser/filebrowser /usr/bin/filebrowser

# copy default caddyfile
COPY Caddyfile /etc/caddy/Caddyfile
COPY .filebrowser.json /etc/filebrowser/.filebrowser.json

ENTRYPOINT ["caddy"]
CMD ["--conf", "/etc/caddy/Caddyfile", "--log", "stdout","--agree=true","filebrowser"]