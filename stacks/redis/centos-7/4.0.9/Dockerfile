FROM govtechstacksg/centos:7

ARG PRODUCT="redis"
ARG PRODUCT_VERSION="4.0.9"
ARG TEMP_DIR_ROOT="/tmp/${PRODUCT}"
ARG TEMP_DIR="${TEMP_DIR_ROOT}/${PRODUCT_VERSION}"
ARG INSTALL_DIR_ROOT="/opt/${PRODUCT}"
ARG INSTALL_DIR="${INSTALL_DIR_ROOT}/${PRODUCT_VERSION}"

ARG BUILD_DIR="${TEMP_DIR}/build"
ARG SOURCE_URL="http://download.redis.io/releases/redis-4.0.9.tar.gz"
ARG SOURCE_FILE="redis-4.0.9.tar.gz"


USER ${ROOT_UID}

RUN \
mkdir -p ${TEMP_DIR} ${INSTALL_DIR} && \
cd ${TEMP_DIR} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_FILE}  ${SOURCE_URL} && \
REDIS_COMPILE_DEPENDENCIES="gcc make" && \
container-yum-install ${REDIS_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${BUILD_DIR} && \
tar --extract  --directory=${BUILD_DIR} --strip-components=1  --file=${SOURCE_FILE}  && \
cd ${BUILD_DIR} && \
make && \
PREFIX=${INSTALL_DIR} make install && \
cd ${INSTALL_DIR} && \
container-fix-ownership ${INSTALL_DIR} && \
container-fix-permissions ${INSTALL_DIR} && \
rm -rf ${TEMP_DIR_ROOT}

ENV REDIS_HOME="${INSTALL_DIR}"
ENV PATH="${REDIS_HOME}/bin:${PATH}"


USER ${APP_UID}

WORKDIR ${REDIS_HOME}

EXPOSE 6379

CMD ${REDIS_HOME}/bin/redis-server
