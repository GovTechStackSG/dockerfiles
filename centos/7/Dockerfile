FROM centos:centos7

ARG APP_UID=1000
ARG APP_GID=0
ARG APP_USERNAME=appuser
ARG APP_GROUPNAME=appuser
ARG APP_USER_HOME=/usr/src/app

ENV ROOT_UID 0
ENV APP_UID ${APP_UID}
ENV APP_GID ${APP_GID}
ENV APP_USERNAME ${APP_USERNAME}
ENV APP_GROUPNAME ${APP_GROUPNAME}
ENV APP_USER_HOME ${APP_USER_HOME}
ENV HOME ${APP_USER_HOME}

USER ${ROOT_UID}

COPY \
scripts \
scripts/container-fix-permissions \
scripts/container-fix-ownership \
scripts/container-yum-install \
scripts/container-import-ca-cert \
/usr/local/bin/

RUN chmod a+rxs \
/usr/local/bin/container-create-user \
/usr/local/bin/container-fix-permissions \
/usr/local/bin/container-fix-ownership \
/usr/local/bin/container-yum-install \
/usr/local/bin/container-import-ca-cert && \
container-yum-install git nano curl openssl && \
container-create-user

USER ${APP_UID}

WORKDIR ${APP_USER_HOME}

CMD /bin/bash
