FROM govtechstacksg/centos:7

ARG PRODUCT="haproxy"
ARG PRODUCT_VERSION="1.8.8"
ARG TEMP_DIR_ROOT="/tmp/${PRODUCT}"
ARG TEMP_DIR="${TEMP_DIR_ROOT}/${PRODUCT_VERSION}"
ARG INSTALL_DIR_ROOT="/opt/${PRODUCT}"
ARG INSTALL_DIR="${INSTALL_DIR_ROOT}/${PRODUCT_VERSION}"

ARG BUILD_DIR="${TEMP_DIR}/build"
ARG SOURCE_URL="http://www.haproxy.org/download/1.8/src/haproxy-1.8.8.tar.gz"
ARG SOURCE_FILE="haproxy-1.8.8.tar.gz"
ARG SOURCE_HASH_URL="http://www.haproxy.org/download/1.8/src/haproxy-1.8.8.tar.gz.md5"
ARG SOURCE_HASH_FILE="haproxy-1.8.8.tar.gz.md5"


USER ${ROOT_UID}

COPY haproxy.conf ${INSTALL_DIR}/

RUN \
set -ex && \
mkdir -p ${TEMP_DIR} ${INSTALL_DIR} && \
cd ${TEMP_DIR} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_FILE}  ${SOURCE_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_HASH_FILE}  ${SOURCE_HASH_URL} && \
grep "${SOURCE_FILE}" ${SOURCE_HASH_FILE} | md5sum --quiet --check - && \
HAPROXY_COMPILE_DEPENDENCIES="gcc make openssl-devel pcre-devel zlib-devel" && \
container-yum-install ${HAPROXY_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${BUILD_DIR} && \
tar --extract  --directory=${BUILD_DIR} --strip-components=1  --file=${SOURCE_FILE}  && \
cd ${BUILD_DIR} && \
make TARGET=linux2628 USE_PCRE=1 USE_ZLIB=1 USE_LINUX_SPLICE=1 USE_LINUX_TPROXY=1 USE_LIBCRYPT=1 && \
make install-bin DESTDIR=${INSTALL_DIR} SBINDIR=/sbin && \
cd ${INSTALL_DIR} && \
container-fix-ownership ${INSTALL_DIR} && \
container-fix-permissions ${INSTALL_DIR} && \
rm -rf ${TEMP_DIR_ROOT}

ENV HAPROXY_HOME="${INSTALL_DIR}"
ENV PATH="${HAPROXY_HOME}/sbin:${PATH}"
ENV HAPROXY_FRONTEND_PORT="8080"
ENV HAPROXY_BACKEND_SERVER1="app"
ENV HAPROXY_BACKEND_PORT="8080"


USER ${APP_UID}

WORKDIR ${HAPROXY_HOME}

CMD ${HAPROXY_HOME}/sbin/haproxy -f ${HAPROXY_HOME}/haproxy.conf
