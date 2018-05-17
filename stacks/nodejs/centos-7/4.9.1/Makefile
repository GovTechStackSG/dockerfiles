SHELL := /bin/bash
.SHELLFLAGS = -exc
.ONESHELL:

REPOSITORY_NAME := govtechstacksg/nodejs
TAGS := 4.9.1-centos-7 4-latest
REGISTRIES := 

PRIMARY_IMAGE_NAME := $(REPOSITORY_NAME):$(shell echo $(TAGS) | tr ' ' '\n' | awk '{print length(), $$0}' | sort -rn | head -n 1 | cut -d ' ' -f2)
CONTAINER_NAME := $(shell echo $(REPOSITORY_NAME) | sed -e 's/[^a-zA-Z0-9_.-]/-/g')
CONTAINER_PORTS := 
CONTAINER_SHELL := bash

.PHONY: all clean build push bash sudo run exec rm rmi
.IGNORE: sudo run rm rmi

define pass_proxy_env_vars
	if [[ ! -z "${http_proxy}" ]]; then $(1)+="--$(2) http_proxy=${http_proxy} "; fi
	if [[ ! -z "${HTTP_PROXY}" ]]; then $(1)+="--$(2) HTTP_PROXY=${HTTP_PROXY} "; fi
	if [[ ! -z "${https_proxy}" ]]; then $(1)+="--$(2) https_proxy=${https_proxy} "; fi
	if [[ ! -z "${HTTPS_PROXY}" ]]; then $(1)+="--$(2) HTTPS_PROXY=${HTTPS_PROXY} "; fi
	if [[ ! -z "${ftp_proxy}" ]]; then $(1)+="--$(2) ftp_proxy=${ftp_proxy} "; fi
	if [[ ! -z "${FTP_PROXY}" ]]; then $(1)+="--$(2) FTP_PROXY=${FTP_PROXY} "; fi
	if [[ ! -z "${no_proxy}" ]]; then $(1)+="--$(2) no_proxy=${no_proxy} "; fi
	if [[ ! -z "${NO_PROXY}" ]]; then $(1)+="--$(2) NO_PROXY=${NO_PROXY} "; fi
endef

define set_docker_run_args
	DOCKER_RUN_ARGS="$${DOCKER_RUN_ARGS} "
	DOCKER_RUN_ARGS+="--tty "
	DOCKER_RUN_ARGS+="--interactive "
	DOCKER_RUN_ARGS+="--rm "
	for CONTAINER_PORT in $(CONTAINER_PORTS); do
		DOCKER_RUN_ARGS+="--publish $${CONTAINER_PORT}:$${CONTAINER_PORT} "
	done
	$(call pass_proxy_env_vars,DOCKER_RUN_ARGS,env)
	DOCKER_RUN_ARGS+="--name $(CONTAINER_NAME) "
endef

build:
	DOCKER_BUILD_ARGS="$${DOCKER_BUILD_ARGS} "
	$(call pass_proxy_env_vars,DOCKER_BUILD_ARGS,build-arg)
	for TAG in $(TAGS); do
		DOCKER_BUILD_ARGS+="--tag $(REPOSITORY_NAME):$${TAG} "
	done
	if [[ ! -z "$(REGISTRIES)" ]]; then
		for REGISTRY in $(REGISTRIES); do
			for TAG in $(TAGS); do
				DOCKER_BUILD_ARGS+="--tag $${REGISTRY}/$(REPOSITORY_NAME):$${TAG} "
			done
		done
	fi
	sudo docker build $${DOCKER_BUILD_ARGS} .

push:
	if [[ ! -z "$(REGISTRIES)" ]]; then
		for REGISTRY in $(REGISTRIES); do
			sudo docker push $${REGISTRY}/$(REPOSITORY_NAME):$${TAG}
		done
	else
		for TAG in $(TAGS); do
			sudo docker push $(REPOSITORY_NAME):$${TAG}
		done
	fi

run:
	$(call set_docker_run_args)
	sudo docker run $${DOCKER_RUN_ARGS} $(PRIMARY_IMAGE_NAME)

$(CONTAINER_SHELL):
	$(call set_docker_run_args)
	sudo docker run $${DOCKER_RUN_ARGS} $(PRIMARY_IMAGE_NAME) $(CONTAINER_SHELL)

sudo:
	$(call set_docker_run_args)
	DOCKER_RUN_ARGS+="--user 0 "
	DOCKER_RUN_ARGS+="--privileged "
	sudo docker run $${DOCKER_RUN_ARGS} $(PRIMARY_IMAGE_NAME) $(CONTAINER_SHELL)

rm:
	DOCKER_RM_ARGS="$${DOCKER_RM_ARGS} --force "
	sudo docker rm $${DOCKER_RM_ARGS} $(CONTAINER_NAME)

rmi:
	DOCKER_RMI_ARGS="$${DOCKER_RMI_ARGS} --force "
	for TAG in $(TAGS); do
		IMAGE_NAMES+="$(REPOSITORY_NAME):$${TAG} "
	done
	if [[ ! -z "$(REGISTRIES)" ]]; then
		for REGISTRY in $(REGISTRIES); do
			for TAG in $(TAGS); do
				IMAGE_NAMES+="$${REGISTRY}/$(REPOSITORY_NAME):$${TAG} "
			done
		done
	fi
	sudo docker rmi $${DOCKER_RMI_ARGS} $${IMAGE_NAMES}

save:
	if [[ -z "IMAGE_NAME" ]]; then
		echo "IMAGE_NAME is empty"
		exit 1
	fi
	ARCHIVE_NAME="$$(echo $${IMAGE_NAME} | sed -e 's/[^a-zA-Z0-9_.-]\+/_/g').sh"
	cat <<-EOF > $${ARCHIVE_NAME}
		#!/bin/bash -ex
		echo Importing $${IMAGE_NAME}
		ARCHIVE=\$$(awk '/^__ARCHIVE_MARKER__/ { print NR + 1; exit 0; }' \$$0)
		tail -n+\$$ARCHIVE \$$0 | sudo docker load
		sudo docker push $${IMAGE_NAME}
		exit 0
		__ARCHIVE_MARKER__
		EOF
	sudo docker tag $(PRIMARY_IMAGE_NAME) $${IMAGE_NAME}
	sudo docker save $${IMAGE_NAME} | gzip -9 >> $${ARCHIVE_NAME}

clean: rm rmi

all: build push
