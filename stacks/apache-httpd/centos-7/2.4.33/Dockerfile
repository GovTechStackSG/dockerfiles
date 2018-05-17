

FROM govtechstacksg/centos:7


ARG PRODUCT="httpd"
ARG VENDOR="apache"
ARG PRODUCT_VERSION="2.4.33"
ARG TEMP_DIR_ROOT="/tmp/${VENDOR}-${PRODUCT}"
ARG TEMP_DIR="${TEMP_DIR_ROOT}/${PRODUCT_VERSION}"
ARG INSTALL_DIR_ROOT="/opt/${VENDOR}-${PRODUCT}"
ARG INSTALL_DIR="${INSTALL_DIR_ROOT}/${PRODUCT_VERSION}"

ARG APR_BUILD_DIR="${TEMP_DIR}/apr/build"
ARG APR_URL="http://www-us.apache.org/dist/apr/apr-1.6.3.tar.bz2"
ARG APR_FILE="apr-1.6.3.tar.bz2"
ARG APR_CHECKSUM_URL="http://www-us.apache.org/dist/apr/apr-1.6.3.tar.bz2.sha256"
ARG APR_CHECKSUM_FILE="apr-1.6.3.tar.bz2.sha256"
ARG APR_GPG_KEYS_URL="http://www-us.apache.org/dist/apr/KEYS"
ARG APR_GPG_KEYS_FILE="APR_KEYS"
ARG APR_SIGNATURE_URL="http://www-us.apache.org/dist/apr/apr-1.6.3.tar.bz2.asc"
ARG APR_SIGNATURE_FILE="apr-1.6.3.tar.bz2.asc"
ARG APR_UTIL_BUILD_DIR="${TEMP_DIR}/apr_util/build"
ARG APR_UTIL_URL="http://www-us.apache.org/dist/apr/apr-util-1.6.1.tar.bz2"
ARG APR_UTIL_FILE="apr-util-1.6.1.tar.bz2"
ARG APR_UTIL_CHECKSUM_URL="http://www-us.apache.org/dist/apr/apr-util-1.6.1.tar.bz2.sha256"
ARG APR_UTIL_CHECKSUM_FILE="apr-util-1.6.1.tar.bz2.sha256"
ARG APR_UTIL_GPG_KEYS_URL="http://www-us.apache.org/dist/apr/KEYS"
ARG APR_UTIL_GPG_KEYS_FILE="APR_UTIL_KEYS"
ARG APR_UTIL_SIGNATURE_URL="http://www-us.apache.org/dist/apr/apr-util-1.6.1.tar.bz2.asc"
ARG APR_UTIL_SIGNATURE_FILE="apr-util-1.6.1.tar.bz2.asc"
ARG BUILD_DIR="${TEMP_DIR}/build"
ARG SOURCE_URL="http://www-us.apache.org/dist/httpd/httpd-2.4.33.tar.bz2"
ARG SOURCE_FILE="httpd-2.4.33.tar.bz2"
ARG CHECKSUM_URL="http://www-us.apache.org/dist/httpd/httpd-2.4.33.tar.bz2.sha256"
ARG CHECKSUM_FILE="httpd-2.4.33.tar.bz2.sha256"
ARG GPG_KEYS_URL="http://www-us.apache.org/dist/httpd/KEYS"
ARG GPG_KEYS_FILE="KEYS"
ARG SIGNATURE_URL="http://www-us.apache.org/dist/httpd/httpd-2.4.33.tar.bz2.asc"
ARG SIGNATURE_FILE="httpd-2.4.33.tar.bz2.asc"


USER ${ROOT_UID}


RUN \
set -ex && \
mkdir -p ${TEMP_DIR} ${INSTALL_DIR} && \
cd ${TEMP_DIR} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_FILE}  ${APR_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_CHECKSUM_FILE}  ${APR_CHECKSUM_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_GPG_KEYS_FILE}  ${APR_GPG_KEYS_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_SIGNATURE_FILE}  ${APR_SIGNATURE_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_UTIL_FILE}  ${APR_UTIL_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_UTIL_CHECKSUM_FILE}  ${APR_UTIL_CHECKSUM_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_UTIL_GPG_KEYS_FILE}  ${APR_UTIL_GPG_KEYS_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${APR_UTIL_SIGNATURE_FILE}  ${APR_UTIL_SIGNATURE_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${SOURCE_FILE}  ${SOURCE_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${CHECKSUM_FILE}  ${CHECKSUM_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${GPG_KEYS_FILE}  ${GPG_KEYS_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${SIGNATURE_FILE}  ${SIGNATURE_URL} && \
gpg --allow-non-selfsigned-uid --import ${APR_GPG_KEYS_FILE} && \
gpg --allow-non-selfsigned-uid --import ${GPG_KEYS_FILE} && \
gpg --verify ${APR_SIGNATURE_FILE} && \
grep "${APR_FILE}" ${APR_CHECKSUM_FILE} | sha256sum --quiet --check - && \
gpg --allow-non-selfsigned-uid --import ${APR_UTIL_GPG_KEYS_FILE} && \
gpg --allow-non-selfsigned-uid --import ${GPG_KEYS_FILE} && \
gpg --verify ${APR_UTIL_SIGNATURE_FILE} && \
grep "${APR_UTIL_FILE}" ${APR_UTIL_CHECKSUM_FILE} | sha256sum --quiet --check - && \
gpg --allow-non-selfsigned-uid --import ${GPG_KEYS_FILE} && \
gpg --verify ${SIGNATURE_FILE} && \
grep "${SOURCE_FILE}" ${CHECKSUM_FILE} | sha256sum --quiet --check - && \
APR_COMPILE_DEPENDENCIES="autoconf libtool gcc-c++ make bzip2 expat-devel pcre-devel" && \
container-yum-install ${APR_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${APR_BUILD_DIR} && \
cd ${TEMP_DIR} && \
tar --extract  --directory=${APR_BUILD_DIR} --strip-components=1  --file=${APR_FILE}  && \
cd ${APR_BUILD_DIR} && \
./configure && \
make && \
make install && \
APR_UTIL_COMPILE_DEPENDENCIES="autoconf libtool gcc-c++ make bzip2 expat-devel pcre-devel" && \
container-yum-install ${APR_UTIL_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${APR_UTIL_BUILD_DIR} && \
cd ${TEMP_DIR} && \
tar --extract  --directory=${APR_UTIL_BUILD_DIR} --strip-components=1  --file=${APR_UTIL_FILE}  && \
cd ${APR_UTIL_BUILD_DIR} && \
./configure --with-apr=/usr/local/apr && \
make && \
make install && \
HTTPD_COMPILE_DEPENDENCIES="autoconf libtool gcc-c++ make bzip2 expat-devel pcre-devel" && \
container-yum-install ${HTTPD_COMPILE_DEPENDENCIES} && \
mkdir -p ${TEMP_DIR} ${BUILD_DIR} && \
cd ${TEMP_DIR} && \
tar --extract  --directory=${BUILD_DIR} --strip-components=1  --file=${SOURCE_FILE}  && \
cd ${BUILD_DIR} && \
./configure --with-apr=/usr/local/apr --prefix=${INSTALL_DIR} && \
make && \
make install && \
container-fix-ownership ${INSTALL_DIR} && \
container-fix-permissions ${INSTALL_DIR} && \
rm -rf ${TEMP_DIR_ROOT} && \
sed -i -e "s/\(Listen[[:blank:]]\+\).*\$/\18080/" ${INSTALL_DIR}/conf/httpd.conf && \
container-yum-install remove ${APR_COMPILE_DEPENDENCIES} ${APR_UTIL_COMPILE_DEPENDENCIES} ${HTTPD_COMPILE_DEPENDENCIES}


ENV HTTPD_HOME="${INSTALL_DIR}"
ENV PATH="${HTTPD_HOME}/bin:${PATH}"


USER ${APP_UID}

WORKDIR ${HTTPD_HOME}

EXPOSE 8080

CMD ${HTTPD_HOME}/bin/httpd -DFOREGROUND
