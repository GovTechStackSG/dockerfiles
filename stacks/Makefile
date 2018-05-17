
MAKEFILES:=$(shell find . -mindepth 2 -name "Makefile")
UPDATE_SCRIPTS:=$(shell find . -mindepth 2 -name "update.py")

.PHONY: all update force build push

all: update build push

build: $(MAKEFILES)
	FAILED_MAKEFILES=""; \
	for MAKEFILE in $(MAKEFILES); do \
		$(MAKE) -C $$(dirname $${MAKEFILE}); \
		if [[ $$? -ne 0 ]]; then FAILED_MAKEFILES+="$${MAKEFILE} "; fi; \
	done; \
	if [[ ! -z "$${FAILED_MAKEFILES}" ]]; then \
		echo "Failed:"; \
		for FAILED_MAKEFILE in $${FAILED_MAKEFILES}; do \
			echo $${FAILED_MAKEFILE}; \
		done; \
		exit 1; \
	fi

push: $(MAKEFILES)
	FAILED_MAKEFILES=""; \
	for MAKEFILE in $(MAKEFILES); do \
		$(MAKE) -C $$(dirname $${MAKEFILE}) push; \
		if [[ $$? -ne 0 ]]; then FAILED_MAKEFILES+="$${MAKEFILE} "; fi; \
	done; \
	if [[ ! -z "$${FAILED_MAKEFILES}" ]]; then \
		echo "Failed:"; \
		for FAILED_MAKEFILE in $${FAILED_MAKEFILES}; do \
			echo $${FAILED_MAKEFILE}; \
		done; \
		exit 1; \
	fi

update:
	FAILED_UPDATE_SCRIPTS=""; \
	for UPDATE_SCRIPT in $(UPDATE_SCRIPTS); do \
		pushd $$(dirname $${UPDATE_SCRIPT}); \
		python2 $$(basename $${UPDATE_SCRIPT}); \
		if [[ $$? -ne 0 ]]; then FAILED_UPDATE_SCRIPTS+="$${UPDATE_SCRIPT} "; fi; \
		popd; \
	done; \
	if [[ ! -z "$${FAILED_UPDATE_SCRIPTS}" ]]; then \
		echo "Failed:"; \
		for FAILED_UPDATE_SCRIPT in $${FAILED_UPDATE_SCRIPTS}; do \
			echo $${FAILED_UPDATE_SCRIPT}; \
		done; \
		exit 1; \
	fi

force:
	FAILED_UPDATE_SCRIPTS=""; \
	for UPDATE_SCRIPT in $(UPDATE_SCRIPTS); do \
		pushd $$(dirname $${UPDATE_SCRIPT}); \
		python2 $$(basename $${UPDATE_SCRIPT}) -f; \
		if [[ $$? -ne 0 ]]; then FAILED_UPDATE_SCRIPTS+="$${UPDATE_SCRIPT} "; fi; \
		popd; \
	done; \
	if [[ ! -z "$${FAILED_UPDATE_SCRIPTS}" ]]; then \
		echo "Failed:"; \
		for FAILED_UPDATE_SCRIPT in $${FAILED_UPDATE_SCRIPTS}; do \
			echo $${FAILED_UPDATE_SCRIPT}; \
		done; \
		exit 1; \
	fi
