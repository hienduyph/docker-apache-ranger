FROM docker.io/maven:3.9.9-eclipse-temurin-8

ARG BUILD_USER
RUN useradd -u ${BUILD_USER} -m -s /bin/bash build 

RUN apt update && apt install python3 curl gcc git -y \
  && curl -fsSL https://deb.nodesource.com/setup_lts.x | bash \
  && apt install nodejs -y

USER ${BUILD_USER}
ENV HOME=/home/build
