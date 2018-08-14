FROM golang:1.10-alpine as build

RUN apk add --no-cache git

# micro
RUN go get -v github.com/micro/micro

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
COPY --from=build /go/bin/micro /usr/bin/micro
RUN /usr/bin/upx --ultra-brute /usr/bin/micro

# test
RUN /usr/bin/micro -version

#
# Final image
#
FROM alpine:3.7

RUN apk add --update ca-certificates && \
    rm -rf /var/cache/apk/* /tmp/*

# copy caddy binary and ca certs
COPY --from=compress /usr/bin/micro /bin/micro

ENTRYPOINT ["/bin/micro"]

