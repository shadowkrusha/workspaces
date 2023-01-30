FROM ghcr.io/shadowkrusha/base-workspace:1.0.8

USER root

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Australia/Perth

RUN apt-get update && apt-get install -y \
    openjdk-17-jdk-headless

# renovate: datasource=github-releases depName=apache/maven
ENV MAVEN_VERSION=3.8.7

RUN mkdir -p /usr/local/maven \
  && cd /usr/local/maven \
  && curl -fsSL https://dlcdn.apache.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
       -o /tmp/apache-maven.tar.gz \
  && tar zxvf /tmp/apache-maven.tar.gz --strip-components=1 \
  && rm /tmp/apache-maven.tar.gz

USER coder

ENV PATH=${PATH}:/usr/local/maven/bin:/home/coder/.local/bin

RUN  code-server \
 --install-extension vscjava.vscode-java-pack \
 --install-extension redhat.fabric8-analytics \
 --install-extension redhat.vscode-xml \
 --install-extension vscjava.vscode-spring-initializr

