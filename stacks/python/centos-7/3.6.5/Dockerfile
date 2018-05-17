FROM govtechstacksg/centos:7


ARG PRODUCT="python3"
ARG PRODUCT_VERSION="3.6.5"
ARG TEMP_DIR_ROOT="/tmp/${PRODUCT}"
ARG TEMP_DIR="${TEMP_DIR_ROOT}/${PRODUCT_VERSION}"
ARG INSTALL_DIR_ROOT="/opt/${PRODUCT}"
ARG INSTALL_DIR="${INSTALL_DIR_ROOT}/${PRODUCT_VERSION}"

ARG BUILD_DIR="${TEMP_DIR}/build"
ARG SOURCE_URL="https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz"
ARG SOURCE_FILE="Python-3.6.5.tar.xz"
ARG GPG_KEYS_URL="https://www.python.org/static/files/pubkeys.txt"
ARG GPG_KEYS_FILE="pubkeys.txt"
ARG SOURCE_SIGNATURE_URL="https://www.python.org/ftp/python/3.6.5/Python-3.6.5.tar.xz.asc"
ARG SOURCE_SIGNATURE_FILE="Python-3.6.5.tar.xz.asc"


USER ${ROOT_UID}


RUN \
mkdir -p ${TEMP_DIR} ${INSTALL_DIR} && \
cd ${TEMP_DIR} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_FILE}  ${SOURCE_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${GPG_KEYS_FILE}  ${GPG_KEYS_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_SIGNATURE_FILE}  ${SOURCE_SIGNATURE_URL} && \
gpg --import ${GPG_KEYS_FILE} && \
gpg --verify ${SOURCE_SIGNATURE_FILE} ${SOURCE_FILE} && \
PYTHON3_COMPILE_DEPENDENCIES="gcc gcc-c++ make zlib-devel openssl-devel readline-devel bzip2-devel xz-devel gdbm-devel tk-devel sqlite-devel" && \
container-yum-install ${PYTHON3_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${BUILD_DIR} && \
tar --extract  --directory=${BUILD_DIR} --strip-components=1  --file=${SOURCE_FILE}  && \
cd ${BUILD_DIR} && \
./configure --enable-optimizations --prefix=${INSTALL_DIR} && \
make && \
make install && \
cd ${INSTALL_DIR} && \
PATH=${PATH}:${INSTALL_DIR}/bin ${INSTALL_DIR}/bin/pip3 install -U pip && \
container-fix-ownership ${INSTALL_DIR} && \
container-fix-permissions ${INSTALL_DIR} && \
rm -rf ${TEMP_DIR_ROOT}


ENV PYTHON3_HOME="${INSTALL_DIR}"
ENV PATH="${PYTHON3_HOME}/bin:${PATH}"


USER ${APP_UID}
