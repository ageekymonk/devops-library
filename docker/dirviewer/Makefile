IMAGE_NAME = dirviewer
IMAGE_VERSION = latest
DOCKER_REGISTRY = docker.io
ORG_NAME = ramz
IMAGE_TAG = $(DOCKER_REGISTRY)/$(ORG_NAME)/$(IMAGE_NAME):$(IMAGE_VERSION)
WORKING_DIR := $(shell pwd)

.PHONY: build tag push

release:: build tag push

push::
	@docker push $(IMAGE_TAG)

build::
	@docker build --pull -t $(IMAGE_TAG) $(WORKING_DIR)
