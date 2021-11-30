FROM golang:1.17-alpine as stage

COPY . /go/src/github.com/microservices-demo/payment/
WORKDIR /go/src/github.com/microservices-demo/payment

RUN GO111MODULE=on go mod init cmd/paymentsvc/main.go
RUN GO111MODULE=on go mod vendor

RUN go build -o /app cmd/paymentsvc/main.go

FROM golang:1.17-alpine

WORKDIR /
COPY --from=stage /app /app

ENV	SERVICE_USER=myuser \
	SERVICE_UID=10001 \
	SERVICE_GROUP=mygroup \
	SERVICE_GID=10001

RUN	addgroup -g ${SERVICE_GID} ${SERVICE_GROUP} && \
	adduser -g "${SERVICE_NAME} user" -D -H -G ${SERVICE_GROUP} -s /sbin/nologin -u ${SERVICE_UID} ${SERVICE_USER} && \
	chmod +x /app && \
    chown -R ${SERVICE_USER}:${SERVICE_GROUP} /app

USER ${SERVICE_USER}

CMD ["/app", "-port=8080"]

EXPOSE 8080