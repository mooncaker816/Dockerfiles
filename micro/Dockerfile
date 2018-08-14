FROM golang:1.10-alpine as build

RUN apk add --no-cache git

# micro
RUN go get -v github.com/micro/micro

RUN /go/bin/micro -version

#
# Final image
#
FROM alpine:3.7

RUN apk add --update ca-certificates && \
    rm -rf /var/cache/apk/* /tmp/*

# copy caddy binary and ca certs
COPY --from=build /go/bin/micro /bin/micro

ENTRYPOINT ["/bin/micro"]

