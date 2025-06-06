# Build image
FROM golang:1.24 AS build-env

ARG gopkg=k8s.io/perf-tests/network/tools/network-policy-enforcement-latency
ADD ["./", "/go/src/$gopkg"]
WORKDIR /go/src/$gopkg/policy-creation-enforcement-latency

RUN go build -o /policy-creation-enforcement-latency

# Runtime image
FROM alpine:3.22
# Need to install curl to send requests to pods
RUN apk --no-cache add curl

COPY --from=build-env /policy-creation-enforcement-latency /policy-creation-enforcement-latency

# Make sure that when compiled on debian, it can run in alpine/musl
RUN mkdir /lib64 && ln -s /lib/libc.musl-x86_64.so.1 /lib64/ld-linux-x86-64.so.2

ENTRYPOINT ["/policy-creation-enforcement-latency"]
