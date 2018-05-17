FROM govtechstacksg/centos:7


ARG PRODUCT="nodejs"
ARG PRODUCT_VERSION="8.11.2"
ARG TEMP_DIR_ROOT="/tmp/${PRODUCT}"
ARG TEMP_DIR="${TEMP_DIR_ROOT}/${PRODUCT_VERSION}"
ARG INSTALL_DIR_ROOT="/opt/${PRODUCT}"
ARG INSTALL_DIR="${INSTALL_DIR_ROOT}/${PRODUCT_VERSION}"

ARG BINARIES_URL="https://nodejs.org/dist/v8.11.2/node-v8.11.2-linux-x64.tar.xz"
ARG BINARIES_FILE="node-v8.11.2-linux-x64.tar.xz"
ARG GPG_KEYS="94AE36675C464D64BAFA68DD7434390BDBE9B9C5 FD3A5288F042B6850C66B31F09FE44734EB7990E 71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 DD8F2338BAE7501E3DD5AC78C273792F7D83545D C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 B9AE9905FFD7803F25714661B63B535A4C206CA9 56730D5401028683275BD23C23EFEFE93C4CFFFE 77984A986EBC2AA786BC0F66B01FBB92821C587A 9554F04D7259F04124DE6B476D5A82AC7E37093B 93C7E9E91B49E432C2F75674B0A78B0A6C481CF6 114F43EE0176B71C7BC219DD50A3051F888C628D 7937DFD2AB06298B2293C3187D33FF9D0246406D"
ARG CHECKSUM_URL="https://nodejs.org/dist/v8.11.2/SHASUMS256.txt.asc"
ARG CHECKSUM_FILE="SHASUMS256.txt.asc"


USER ${ROOT_UID}


RUN \
set -ex && \
mkdir -p ${TEMP_DIR} ${INSTALL_DIR} && \
cd ${TEMP_DIR} && \
curl --silent --show-error --location --junk-session-cookies --output ${BINARIES_FILE}  ${BINARIES_URL} && \
curl --silent --show-error --location --junk-session-cookies --output ${CHECKSUM_FILE}  ${CHECKSUM_URL} && \
gpg --keyserver hkps.pool.sks-keyservers.net --recv-keys ${GPG_KEYS} || \
gpg --keyserver keyserver.pgp.com --recv-keys ${GPG_KEYS} || \
gpg --keyserver pgp.mit.edu --recv-keys ${GPG_KEYS} || \
gpg --keyserver keys.gnupg.net --recv-keys ${GPG_KEYS} && \
gpg --verify ${CHECKSUM_FILE} && \
grep "${BINARIES_FILE}" ${CHECKSUM_FILE} | sha256sum --quiet --check - && \
tar --extract  --directory=${INSTALL_DIR} --strip-components=1  --file=${BINARIES_FILE}  && \
cd ${INSTALL_DIR} && \
PATH=${PATH}:${INSTALL_DIR}/bin ${INSTALL_DIR}/bin/npm install npm@latest -g && \
container-fix-ownership ${INSTALL_DIR} && \
container-fix-permissions ${INSTALL_DIR} && \
rm -rf ${TEMP_DIR_ROOT}


ENV NODEJS_HOME="${INSTALL_DIR}"
ENV PATH="${NODEJS_HOME}/bin:${PATH}"


USER ${APP_UID}
