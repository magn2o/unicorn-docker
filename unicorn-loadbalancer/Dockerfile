FROM alpine:3.9
MAINTAINER magn2o

ARG S6_OVERLAY_VERSION="1.22.1.0"

RUN \
  apk add --no-cache curl npm git && \
  curl -J -L https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz | tar zxvf - -C / && \
  git -C /opt clone https://github.com/UnicornTranscoder/UnicornLoadBalancer.git && \
  npm install --prefix=/opt/UnicornLoadBalancer && \
  git -C /opt clone https://github.com/UnicornTranscoder/UnicornFFMPEG.git && \
  apk del git && \
  rm -rf /var/cache/apk/* /tmp/* /media/*

COPY rootfs/ /

EXPOSE 3001/tcp

ENTRYPOINT ["/init"]
