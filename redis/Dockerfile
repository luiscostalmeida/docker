# Dockerfile for Redis on Alpine with prometheus' style /metrics endpoint
FROM alpine:edge
MAINTAINER Tiago Caxias http://github.com/tcaxias
WORKDIR /app

ENV \
    EXPORTER="github.com/oliver006/redis_exporter" \
    CONF="daemonize no |save \"''\"|" \
    PKGS="supervisor redis" \
    DEPS="git go build-base" \
    GOPATH="/tmp/go" \
    PASSWD=""

RUN \
    apk --no-cache --update add $PKGS $DEPS && \
    go get $EXPORTER && \
    mv $GOPATH/bin/* /app/ && \
    apk --no-cache del $DEPS && \
    rm -rf $GOPATH

ADD \
    supervisord.conf \
    start_redis.sh \
    start_redis_exporter.sh \
    /app/

RUN chmod +x /app/*.sh

EXPOSE 6379 9104
CMD [ "supervisord", "-c", "/app/supervisord.conf", "-j", "/dev/shm/supervisor.pid", "-l", "/dev/null" ]
ENV TERM=xterm
