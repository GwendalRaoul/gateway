FROM alpine:3.12.0

RUN adduser --disabled-password wirepas

RUN apk add --no-cache dbus 

COPY ./sink_service/com.wirepas.sink.conf /etc/dbus-1/system.d/
COPY ./docker/dbus/entrypoint.sh /etc/init/

#USER wirepas

ENTRYPOINT ["/etc/init/entrypoint.sh"]
