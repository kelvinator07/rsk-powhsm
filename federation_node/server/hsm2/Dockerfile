FROM debian:bullseye-slim

WORKDIR /hsm

RUN mkdir -p /hsmbin
COPY bin/manager.tgz /hsmbin/manager.tgz
RUN mkdir -p /hsmbin/manager && \
    tar -xzmf /hsmbin/manager.tgz -C /hsmbin/manager && \
    rm -f /hsmbin/manager.tgz

CMD /hsmbin/manager/manager -b 0.0.0.0
