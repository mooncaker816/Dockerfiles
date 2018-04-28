FROM golang:1.10-alpine as build

ARG CADDY_VERSION="0.10.14"
ARG PLUGINS=""

RUN apk add --no-cache git openssh-client 

# caddy
RUN git clone https://github.com/mholt/caddy -b "v${CADDY_VERSION}" /go/src/github.com/mholt/caddy \
    && cd /go/src/github.com/mholt/caddy \
    && git checkout -b "v${CADDY_VERSION}"

# plugin helper
RUN go get -v github.com/abiosoft/caddyplug/caddyplug

# plugins
RUN for plugin in $(echo $PLUGINS | tr "," " "); do \
    go get -v $(caddyplug package $plugin); \
    printf "package caddyhttp\nimport _ \"$(caddyplug package $plugin)\"" > \
        /go/src/github.com/mholt/caddy/caddyhttp/$plugin.go ; \
    done

# builder dependency
RUN git clone https://github.com/caddyserver/builds /go/src/github.com/caddyserver/builds

# build
RUN cd /go/src/github.com/mholt/caddy/caddy \
    && git checkout -f \
    && go run build.go \
    && mv caddy /go/bin

# Hugo
ARG HUGO_VERSION="0.40.1"
ENV HUGO_BINARY hugo_${HUGO_VERSION}_linux-64bit

# Install pygments (for syntax highlighting) and bash
RUN apk update && apk add py-pygments && apk add bash && rm -rf /var/cache/apk/*

# Download and Install hugo
RUN mkdir /usr/local/hugo
ADD https://github.com/spf13/hugo/releases/download/v${HUGO_VERSION}/${HUGO_BINARY}.tar.gz /usr/local/hugo/
RUN tar xzf /usr/local/hugo/${HUGO_BINARY}.tar.gz -C /usr/local/hugo/ \
	&& rm /usr/local/hugo/${HUGO_BINARY}.tar.gz


#
# Compress Caddy with upx
#
FROM debian:stable as compress

# curl, tar
RUN apt-get update && apt install -y --no-install-recommends \
    tar \
    xz-utils \
    curl \
    ca-certificates

# get official upx binary
RUN curl --silent --show-error --fail --location -o - \
    "https://github.com/upx/upx/releases/download/v3.94/upx-3.94-amd64_linux.tar.xz" \
    | tar --no-same-owner -C /usr/bin/ -xJ \
    --strip-components 1 upx-3.94-amd64_linux/upx

# copy and compress
COPY --from=build /go/bin/caddy /usr/bin/caddy
COPY --from=build /usr/local/hugo/hugo /usr/bin/hugo
RUN /usr/bin/upx --ultra-brute /usr/bin/caddy /usr/bin/hugo

# test
RUN /usr/bin/caddy -version
RUN /usr/bin/caddy -plugins
RUN /usr/bin/hugo version


#
# Final image
#
FROM scratch

# copy caddy binary and ca certs
COPY --from=compress /usr/bin/caddy /bin/caddy
COPY --from=compress /usr/bin/hugo /bin/hugo

# copy default caddyfile
COPY Caddyfile /etc/Caddyfile

# serve from /srv
WORKDIR /srv
COPY index.html /srv/index.html

ENTRYPOINT ["/bin/caddy"]
CMD ["--conf", "/etc/Caddyfile", "--log", "stdout","--agree=true"]

# Build-time metadata as defined at http://label-schema.org
ARG BUILD_DATE
ARG VCS_REF
ARG VCS_URL
ARG VERSION
LABEL org.label-schema.build-date=$BUILD_DATE \
      org.label-schema.name="Hugo & Caddy" \
      org.label-schema.description="Hugo-${HUGO_VERSION} + Caddy:git,hugo-${CADDY_VERSION}" \
      org.label-schema.url="https://gohugo.io/" \
      org.label-schema.vcs-ref=$VCS_REF \
      org.label-schema.vcs-url=$VCS_URL \
      org.label-schema.version=$VERSION \
      org.label-schema.schema-version="1.0"
