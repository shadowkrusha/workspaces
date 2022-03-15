FROM ghcr.io/shadowkrusha/base-workspace:1.0.2

USER root

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Australia/Perth

RUN apt-get update && apt-get install -y \
    openjdk-17-jdk-headless \
    maven

USER coder

ENV PATH=${PATH}:/home/coder/.local/bin

RUN  code-server \
 --install-extension vscjava.vscode-java-pack \
 --install-extension redhat.fabric8-analytics \
 --install-extension redhat.vscode-xml \
 --install-extension vscjava.vscode-spring-initializr

