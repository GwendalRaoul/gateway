FROM alpine:3.12.0 AS builder

RUN apk add --no-cache curl

RUN curl -LsO https://raw.githubusercontent.com/wirepas/gateway/master/sink_service/com.wirepas.sink.conf 

FROM alpine:3.12.0

RUN adduser --disabled-password wirepas

RUN apk add --no-cache dbus 

COPY --from=builder com.wirepas.sink.conf /etc/dbus-1/system.d/
COPY entrypoint.sh /etc/init/

#USER wirepas

ENTRYPOINT ["/etc/init/entrypoint.sh"]
