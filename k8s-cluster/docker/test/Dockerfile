### Builder
FROM golang:1.13-alpine as builder

RUN apk update \
    && apk add --virtual build-dependencies \
        build-base \
        gcc \
        wget \
        git \
    && apk add \
        bash

WORKDIR /usr/src/app
RUN git clone https://github.com/kubernetes/sample-controller.git

WORKDIR /usr/src/app/sample-controller
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build 

### Make executable image
FROM scratch

COPY config /config
COPY --from=builder /usr/src/app/sample-controller/sample-controller /sample-controller

CMD ["/sample-controller", "-kubeconfig", "/config"]
