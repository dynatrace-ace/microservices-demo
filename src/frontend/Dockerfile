FROM golang:1.12-alpine as builder
RUN apk add --no-cache ca-certificates gcc libc-dev git 

ENV PROJECT github.com/GoogleCloudPlatform/microservices-demo/src/frontend
WORKDIR /go/src/$PROJECT

# restore dependencies
COPY Gopkg.* ./

COPY . .
RUN go get -d ./...
RUN go install -ldflags '-linkmode external' .

FROM alpine as release
RUN apk add --no-cache ca-certificates \
    busybox-extras net-tools bind-tools
WORKDIR /frontend
COPY --from=builder /go/bin/frontend /frontend/server
COPY ./templates ./templates
COPY ./static ./static
EXPOSE 8080
ENTRYPOINT ["/frontend/server"]
