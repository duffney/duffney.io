FROM golang:bookworm AS builder

WORKDIR $GOPATH/src/mypackage/myapp/
COPY . .

RUN go get -d -v
RUN go build -o /go/bin/hello

FROM scratch

COPY --from=builder /var/lib/dpkg/status /var/lib/dpkg/status
COPY --from=builder /var/lib/dpkg/available /var/lib/dpkg/available
COPY --from=builder /var/lib/dpkg/info /var/lib/dpkg/info
COPY --from=builder /etc/debian_version /etc/debian_version
COPY --from=builder /go/bin/hello /go/bin/hello

ENTRYPOINT ["/go/bin/hello"]
