FROM docker.io/eclipse-temurin:8-jre

RUN apt update && apt install python3 curl --no-install-recommends lsb-release -y \
  && rm -rf /var/lib/{apt,dpkg,cache,log}/ \
  && userdel "$(id -nu 1000)" || true  \
  && useradd ranger -u 1000 -s /bin/bash -m -d /home/ranger

ARG VERSION
ARG REVISION
ENV RANGER_VERSION=${VERSION}-${REVISION}
ENV RANGER_HOME=/opt/ranger
ENV RANGER_DIST=/home/ranger/dist
ENV RANGER_SCRIPTS=/home/ranger/scripts

COPY ./build-src/ranger/target/version                                  ${RANGER_DIST}/
COPY ./build-src/ranger/target/ranger-${RANGER_VERSION}-usersync.tar.gz ${RANGER_DIST}/
COPY ./ranger-usersync/ranger-usersync.sh                               ${RANGER_SCRIPTS}/

RUN mkdir -p /var/run/ranger /var/log/ranger/usersync /etc/ranger ${RANGER_HOME}/usersync \
  && tar xvfz ${RANGER_DIST}/ranger-${RANGER_VERSION}-usersync.tar.gz --directory=${RANGER_HOME}/usersync --strip-components=1 \
  && rm -f ${RANGER_DIST}/ranger-${RANGER_VERSION}-usersync.tar.gz \
  && mkdir -p ${RANGER_HOME}/usersync/logs/  \
  && ln -s /usr/bin/python3 /usr/bin/python \
  && touch /etc/init.d/ranger-usersync ${RANGER_HOME}/usersync/logs/auth.log \
  && ln -s /etc/init.d/ranger-usersync /etc/rc2.d/S99ranger-usersync \
  && ln -s /etc/init.d/ranger-usersync /etc/rc2.d/K00ranger-usersync \
  && ln -s /etc/init.d/ranger-usersync /etc/rc3.d/S99ranger-usersync \
  && ln -s /etc/init.d/ranger-usersync /etc/rc3.d/K00ranger-usersync \
  && ln -s ${RANGER_HOME}/usersync/ranger-usersync-services.sh /usr/bin/ranger-usersync \
  && chown -R ranger:ranger ${RANGER_HOME}/ ${RANGER_SCRIPTS}/ /var/run/ranger/ /var/log/ranger/ /etc/ranger /etc/init.d/ranger-usersync \
  && chmod 744 ${RANGER_SCRIPTS}/ranger-usersync.sh

USER ranger

WORKDIR ${RANGER_HOME}/usersync

CMD [ "/home/ranger/scripts/ranger-usersync.sh" ]

