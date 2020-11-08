FROM alpine:3.6

LABEL description="Docker container for running your own Syncthing Relay Server."

ARG VERSION=v1.8.0

ENV BUILD_HOST=https://github.com/syncthing/relaysrv/releases/download/${VERSION}
ENV BUILD_FILE_NAME=strelaysrv-linux-amd64-${VERSION}
ENV RELAY_ROOT=/workspace


RUN apk add --update libc6-compat libstdc++ \
  && apk upgrade \
  && apk add --no-cache --virtual build ca-certificates wget \
  && update-ca-certificates \
  && wget -P /tmp ${BUILD_HOST}/${BUILD_FILE_NAME}.tar.gz \
  && tar xvf /tmp/${BUILD_FILE_NAME}.tar.gz -C /tmp \
  && mv /tmp/${BUILD_FILE_NAME}/strelaysrv /usr/local/bin/strelaysrv \
  && addgroup -g 1000 strelaysrv \
  && adduser -D -G strelaysrv -u 1000 strelaysrv \
  && mkdir -p ${RELAY_ROOT} \
  && chown strelaysrv:strelaysrv ${RELAY_ROOT} \
  && apk del build \
  && rm "/tmp/*" -rf


VOLUME $RELAY_ROOT

EXPOSE 22067
EXPOSE 22070

WORKDIR $RELAY_ROOT

USER strelaysrv

CMD ["sh","-c","strelaysrv -keys ${RELAY_ROOT} -pools=\"${POOLS}\""]
