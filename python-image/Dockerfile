FROM ghcr.io/shadowkrusha/base-workspace:sha-d3740d1

USER root

RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip

USER coder

ENV PATH=${PATH}:/home/coder/.local/bin

RUN  code-server --install-extension ms-toolsai.jupyter \
  && code-server --install-extension ms-python.python

# RUN echo 'export PATH=${PATH}:${HOME}/.local/bin' >> /home/coder/.bashrc

COPY requirements.txt /tmp

RUN pip3 install -r /tmp/requirements.txt

# sqlalchemy snowflake-sqlalchemy great_expectations 
# dbt-core dbt-snowflake 