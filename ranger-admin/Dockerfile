FROM docker.io/eclipse-temurin:8-jre

RUN apt update && apt install python3 curl --no-install-recommends lsb-release -y \
  && rm -rf /var/lib/{apt,dpkg,cache,log}/ \
  && userdel "$(id -nu 1000)" || true  \
  && useradd ranger -u 1000 -s /bin/bash -m -d /home/ranger

ARG VERSION
ARG REVISION
ENV RANGER_HOME=/opt/ranger
ENV RANGER_DIST=/home/ranger/dist
ENV RANGER_SCRIPTS=/home/ranger/scripts
ENV RANGER_VERSION=${VERSION}-${REVISION}


RUN mkdir -p /var/run/ranger /var/log/ranger /usr/share/java/ ${RANGER_HOME}/admin ${RANGER_SCRIPTS} \
    && curl -Lo /usr/share/java/postgresql.jar https://jdbc.postgresql.org/download/postgresql-42.7.3.jar \
    && curl -Lo /usr/share/java/mysql.jar https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/9.0.0/mysql-connector-j-9.0.0.jar


COPY ./build-src/ranger/target/version                               ${RANGER_DIST}/
COPY ./build-src/ranger/target/ranger-${RANGER_VERSION}-admin.tar.gz ${RANGER_DIST}/
COPY ranger-admin/ranger.sh                                          ${RANGER_SCRIPTS}/

RUN tar xvfz /home/ranger/dist/ranger-${RANGER_VERSION}-admin.tar.gz --directory=${RANGER_HOME}/admin --strip-components=1 \
  && rm -f /home/ranger/dist/ranger-${RANGER_VERSION}-admin.tar.gz \
  && mkdir -p ${RANGER_HOME}/admin/ews/webapp/WEB-INF/classes/ranger-plugins/starrocks ${RANGER_HOME}/admin/ews/logs \
  && cd ${RANGER_HOME}/admin/ews/webapp/WEB-INF/classes/ranger-plugins/starrocks \
  && curl -LO https://repo1.maven.org/maven2/com/mysql/mysql-connector-j/9.0.0/mysql-connector-j-9.0.0.jar \
  && curl -LO https://releases.starrocks.io/resources/ranger-starrocks-plugin-3.0.0-SNAPSHOT.jar \
  && chown -R ranger:ranger ${RANGER_HOME}/ ${RANGER_SCRIPTS}/ /var/run/ranger/ /var/log/ranger/ /etc/rc2.d/ /etc/init.d/ 


USER ranger

WORKDIR ${RANGER_HOME}/admin

CMD [ "/home/ranger/scripts/ranger.sh" ]
