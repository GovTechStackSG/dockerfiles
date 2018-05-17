FROM govtechstacksg/centos:7


ARG PRODUCT="ruby"
ARG PRODUCT_VERSION="2.5.1"
ARG TEMP_DIR_ROOT="/tmp/${PRODUCT}"
ARG TEMP_DIR="${TEMP_DIR_ROOT}/${PRODUCT_VERSION}"
ARG INSTALL_DIR_ROOT="/opt/${PRODUCT}"
ARG INSTALL_DIR="${INSTALL_DIR_ROOT}/${PRODUCT_VERSION}"

ARG BUILD_DIR="${TEMP_DIR}/build"
ARG SOURCE_URL="http://cache.ruby-lang.org/pub/ruby/ruby-2.5.1.tar.bz2"
ARG SOURCE_FILE="ruby-2.5.1.tar.bz2"


USER ${ROOT_UID}


RUN \
mkdir -p ${TEMP_DIR} ${INSTALL_DIR} && \
cd ${TEMP_DIR} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_FILE}  ${SOURCE_URL} && \
RUBY_COMPILE_DEPENDENCIES="gcc gcc-c++ make zlib-devel gdbm-devel openssl-devel readline-devel bzip2" && \
container-yum-install ${RUBY_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${BUILD_DIR} && \
tar --extract  --directory=${BUILD_DIR} --strip-components=1  --file=${SOURCE_FILE}  && \
cd ${BUILD_DIR} && \
./configure --prefix=${INSTALL_DIR} && \
make && \
make install && \
cd ${INSTALL_DIR} && \
PATH=${PATH}:${INSTALL_DIR}/bin gem update --system && \
container-fix-ownership ${INSTALL_DIR} && \
container-fix-permissions ${INSTALL_DIR} && \
rm -rf ${TEMP_DIR_ROOT}


ENV RUBY_HOME="${INSTALL_DIR}"
ENV PATH="${RUBY_HOME}/bin:${PATH}"


USER ${APP_UID}
