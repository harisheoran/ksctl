FROM golang:1.21 as builder

WORKDIR /app

LABEL MAINTAINER="dipankar das"

COPY . .

RUN go mod tidy

RUN cd httpserver && CGO_ENABLED=0 go build -v -o ksctlserver . && mv ksctlserver ..


FROM alpine

LABEL MAINTAINER="dipankar das"

RUN apk add openssh

RUN adduser --disabled-password -s /bin/sh -u 1000 ksctl

USER ksctl

WORKDIR /app

COPY --from=builder /app/ksctlserver /app/ksctlserver


RUN mkdir -p /app/ksctl-data/config/civo/ha && \
	mkdir -p /app/ksctl-data/config/azure/ha

ENTRYPOINT [ "/app/ksctlserver" ]

EXPOSE 8080
