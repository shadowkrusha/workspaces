FROM ubuntu:22.04

ARG TARGETPLATFORM

RUN apt-get update && apt-get install -y \
    openssl \
    net-tools \
    git \
    locales \
    sudo \
    dumb-init \
    vim \
    curl \
    wget \
    bash-completion


# renovate: datasource=github-releases depName=coder/code-server
ENV CODE_SERVER_VERSION=4.9.1

# renovate: datasource=github-releases depName=boxboat/fixuid
ENV FIXUID_VERSION=0.5.1

RUN \
  case ${TARGETPLATFORM} in \
    "linux/amd64") DOWNLOAD_ARCH="amd64" ;; \
    "linux/arm64") DOWNLOAD_ARCH="arm64" ;; \
  esac \
  && curl -fsSL https://github.com/coder/code-server/releases/download/v${CODE_SERVER_VERSION}/code-server_${CODE_SERVER_VERSION}_${DOWNLOAD_ARCH}.deb \
     -o code-server_${CODE_SERVER_VERSION}_${DOWNLOAD_ARCH}.deb \
  && dpkg -i code-server_${CODE_SERVER_VERSION}_${DOWNLOAD_ARCH}.deb \
  && rm code-server_${CODE_SERVER_VERSION}_${DOWNLOAD_ARCH}.deb \ 
  && curl -sSL "https://github.com/boxboat/fixuid/releases/download/v${FIXUID_VERSION}/fixuid-${FIXUID_VERSION}-linux-${DOWNLOAD_ARCH}.tar.gz" | tar -C /usr/local/bin -xzf - \
  && chown root:root /usr/local/bin/fixuid \
  && chmod 4755 /usr/local/bin/fixuid \
  && mkdir -p /etc/fixuid \
  && printf "user: coder\ngroup: coder\n" > /etc/fixuid/config.yml

## User account
RUN adduser --disabled-password --gecos '' coder && \
    adduser coder sudo && \
    echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers;

RUN chmod g+rw /home && \
    mkdir -p /home/coder/workspace && \
    chown -R coder:coder /home/coder && \
    chown -R coder:coder /home/coder/workspace;

USER coder

RUN git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && \
        ~/.fzf/install

ENV PASSWORD=${PASSWORD:-P@ssw0rd}

RUN echo 'export PS1="\[\e]0;\u@\h: \w\a\]\[\033[01;32m\]\u\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ "' >> /home/coder/.bashrc

WORKDIR /home/coder/workspace

EXPOSE 8080

ENTRYPOINT ["dumb-init", "fixuid", "-q", "/usr/bin/code-server", "--bind-addr", "0.0.0.0:8080", "."]