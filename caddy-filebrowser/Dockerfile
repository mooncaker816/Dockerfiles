FROM alpine

RUN apk add --no-cache bash curl ca-certificates

# caddy
RUN curl https://getcaddy.com | bash -s personal "${PLUGINS}"

# Download and Install filebrowser
RUN mkdir /usr/local/filebrowser
ADD https://github.com/filebrowser/filebrowser/releases/download/v2.0.12/linux-amd64-filebrowser.tar.gz /usr/local/filebrowser/
RUN tar xzf /usr/local/filebrowser/linux-amd64-filebrowser.tar.gz filebrowser -C /usr/local/bin/ \
	&& rm /usr/local/filebrowser/linux-amd64-filebrowser.tar.gz

# copy default caddyfile
COPY Caddyfile /etc/Caddyfile
COPY .filebrowser.json /etc/filebrowser/.filebrowser.json
COPY entrypoint.sh /entrypoint.sh
RUN ["chmod","+x","/entrypoint.sh"]

ENTRYPOINT ["/entrypoint.sh"]