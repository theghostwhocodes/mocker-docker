FROM golang:1.10.3-stretch as builder

LABEL maintainer="paolo.ferretti@gmail.com"

ENV MOCKER_VERSION 0.3.1
ENV MOCKER_DOWNLOAD_URL https://github.com/theghostwhocodes/mocker-go/archive/v0.3.1.zip

RUN apt-get update && apt-get install unzip
RUN mkdir -p /go/src/github.com/theghostwhocodes; \
    wget -P /go/src/github.com/theghostwhocodes/ $MOCKER_DOWNLOAD_URL; \
    unzip -d /go/src/github.com/theghostwhocodes/ /go/src/github.com/theghostwhocodes/v$MOCKER_VERSION.zip; \
    mv /go/src/github.com/theghostwhocodes/mocker-go-$MOCKER_VERSION /go/src/github.com/theghostwhocodes/mocker-go; \
    go build -ldflags "-X main.version=$MOCKER_VERSION" -o /go/src/github.com/theghostwhocodes/mocker-go/build/mocker /go/src/github.com/theghostwhocodes/mocker-go/cmd/mocker/main.go


FROM alpine:3.7

ENV MOCKER_HOST 0.0.0.0
ENV MOCKER_PORT 2626

LABEL maintainer="paolo.ferretti@gmail.com"

VOLUME /data

COPY --from=builder /go/src/github.com/theghostwhocodes/mocker-go/build/mocker /usr/bin/mocker

CMD mocker -host $MOCKER_HOST -port $MOCKER_PORT -data /data