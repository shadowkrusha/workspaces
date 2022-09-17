FROM ghcr.io/shadowkrusha/base-workspace:1.0.6

ARG TARGETPLATFORM

USER root

RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip

# renovate: datasource=github-releases depName=yannh/redis-dump-go
ENV REDIS_DUMP_VERSION=0.8.0

RUN \
  case ${TARGETPLATFORM} in \
    "linux/amd64") DOWNLOAD_ARCH="amd64" ;; \
    "linux/arm64") DOWNLOAD_ARCH="arm64" ;; \
  esac \
  && cd /usr/local/bin \
  && curl -fsSL https://github.com/yannh/redis-dump-go/releases/download/v${REDIS_DUMP_VERSION}/redis-dump-go-linux-${DOWNLOAD_ARCH}.tar.gz \
     -o redis-dump-go-linux-${DOWNLOAD_ARCH}.tar.gz \
  && tar zxvf redis-dump-go-linux-${DOWNLOAD_ARCH}.tar.gz redis-dump-go \
  && rm redis-dump-go-linux-${DOWNLOAD_ARCH}.tar.gz \
  && chown root:root redis-dump-go \
  && chmod 755 redis-dump-go

# renovate: datasource=github-releases depName=mylxsw/redis-tui
ENV REDIS_TUI_VERSION=0.1.6

RUN \
  case ${TARGETPLATFORM} in \
    "linux/amd64") DOWNLOAD_ARCH="linux" ;; \
    "linux/arm64") DOWNLOAD_ARCH="linux-arm" ;; \
  esac \
  && cd /usr/local/bin \
  && curl -fsSL https://github.com/mylxsw/redis-tui/releases/download/${REDIS_TUI_VERSION}/redis-tui-${DOWNLOAD_ARCH} \
     -o redis-tui \
  && chown root:root redis-tui \
  && chmod 755 redis-tui

RUN \
  curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash - \
  && apt-get install -y nodejs \
  && npm install -g redis-commander

USER coder

COPY requirements.txt /tmp

RUN pip3 install -r /tmp/requirements.txt
