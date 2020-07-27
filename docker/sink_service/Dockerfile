FROM debian:buster AS builder

RUN useradd -ms /bin/bash wirepas
RUN apt-get update

RUN apt-get install -y \
        make \
        gcc \
        git \
        libsystemd-dev

USER wirepas
WORKDIR /home/wirepas

COPY --chown=wirepas ./sink_service /home/wirepas/sink_service

WORKDIR /home/wirepas/sink_service

# Todo fix the version
RUN git clone https://github.com/wirepas/c-mesh-api.git

RUN make

FROM debian:buster-slim

RUN useradd -ms /bin/bash wirepas
RUN usermod -a -G dialout wirepas

# Copy the built service and its dependencies from builder
COPY --from=builder /home/wirepas/sink_service/build/sinkService /home/wirepas/

USER wirepas

CMD ["/home/wirepas/sinkService"]
