FROM golang:alpine

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.shu.edu.cn/g' /etc/apk/repositories

ENV TZ=Asia/Shanghai
RUN apk update && apk --no-cache add tzdata ca-certificates strace && \
    ln -sf /usr/share/zoneinfo/${TZ} /etc/localtime && \
    echo "${TZ}" > /etc/timezone