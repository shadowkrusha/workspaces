FROM ghcr.io/shadowkrusha/base-workspace:1.0.6

ARG TARGETPLATFORM

USER root

RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    unzip # Required for SnowSQL

USER coder

ENV PATH=${PATH}:/home/coder/.local/bin

RUN  code-server --install-extension ms-toolsai.jupyter \
  && code-server --install-extension ms-python.python

# RUN echo 'export PATH=${PATH}:${HOME}/.local/bin' >> /home/coder/.bashrc

COPY requirements.txt /tmp

RUN pip3 install -r /tmp/requirements.txt

ENV SNOWSQL_VERSION=1.2.21

RUN \
  case ${TARGETPLATFORM} in \
    "linux/amd64") curl -fsSL https://sfc-repo.snowflakecomputing.com/snowsql/bootstrap/1.2/linux_x86_64/snowsql-${SNOWSQL_VERSION}-linux_x86_64.bash \
        -o /tmp/snowsql.bash; \
        SNOWSQL_DEST=${HOME}/bin SNOWSQL_LOGIN_SHELL=${HOME}/.bashrc bash /tmp/snowsql.bash ;; \
  esac