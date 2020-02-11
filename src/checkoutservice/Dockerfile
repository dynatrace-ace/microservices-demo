FROM golang:1.12-alpine as builder
RUN apk add --no-cache ca-certificates gcc libc-dev git

ENV PROJECT github.com/GoogleCloudPlatform/microservices-demo/src/checkoutservice
WORKDIR /go/src/$PROJECT

# restore dependencies
COPY Gopkg.* ./

COPY . .
RUN go get -d ./...
RUN go build -ldflags '-linkmode external' -gcflags='-N -l' -o /checkoutservice .

FROM alpine as release
RUN apk add --no-cache ca-certificates
RUN GRPC_HEALTH_PROBE_VERSION=v0.2.0 && \
    wget -qO/bin/grpc_health_probe https://github.com/grpc-ecosystem/grpc-health-probe/releases/download/${GRPC_HEALTH_PROBE_VERSION}/grpc_health_probe-linux-amd64 && \
    chmod +x /bin/grpc_health_probe
COPY --from=builder /checkoutservice /checkoutservice
EXPOSE 5050
ENTRYPOINT ["/checkoutservice"]
