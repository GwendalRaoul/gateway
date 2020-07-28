FROM python:3.7.7-buster AS transport_builder_base

RUN useradd -ms /bin/bash wirepas
RUN apt-get update

RUN apt-get install -y \
	python3-dev \
        python3-gi \
        libsystemd-dev \
        libgirepository1.0-dev 

USER wirepas

RUN python3 -m pip install --no-cache-dir --upgrade pip \
    && python3 -m pip install wheel setuptools

RUN pip3 install PyGObject --user

# Split builder in two parts to not rebuild base every time
FROM transport_builder_base AS builder

COPY --chown=wirepas ./python_transport /home/wirepas/python_transport
WORKDIR /home/wirepas/python_transport

RUN ./utils/generate_wheel.sh


# HACK start: avoid installing grpcio (not needed)
# It should be an option of wirepas-messaging wheel instead

# Install wirepas-messaging manually to avoid installing grpcio
RUN pip3 install protobuf==3.10.0 --user
RUN pip3 install --no-deps wirepas_messaging==1.2.0 --user

# Install manually gateway too to avoid installing grpcio again
# Remove wirepa-messaging from deps
RUN sed -i.bak '/wirepas_messaging/d' requirements.txt
RUN pip3 install -r requirements.txt --no-deps dist/wirepas_gateway*.whl --user


FROM python:3.7.7-slim-buster

RUN useradd -ms /bin/bash wirepas

RUN apt-get update \
    && apt-get install -y python3-gi \
    && rm -fr /var/libapt/lists/*

USER wirepas

ENV PATH="/home/wirepas/.local/bin:${PATH}"

# Copy the built wheel and its dependencies from builder
COPY --from=builder /home/wirepas/.local /home/wirepas/.local

CMD ["wm-gw"]
