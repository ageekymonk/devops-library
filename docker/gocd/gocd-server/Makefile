SHELL := /bin/bash
WORKING_DIR := $(shell pwd)

DOCKER_REGISTRY = hub.docker.com
IMAGE_NAME = gocd-server
IMAGE_VERSION = latest
ORG_NAME = ramz
IMAGE_TAG = $(DOCKER_REGISTRY)/$(ORG_NAME)/$(IMAGE_NAME):$(IMAGE_VERSION)

.PHONY: build push

release:: build push

push::
	@docker push $(IMAGE_TAG)

build::
	@docker build --pull -t $(IMAGE_TAG) \
								$(WORKING_DIR)
