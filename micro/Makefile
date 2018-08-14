default: docker_build

# Build Docker image
build: docker_build output

# Build and push Docker image
release: docker_build docker_push output

VERSION = 0.13.0

# Image and binary can be overidden with env vars.
DOCKER_IMAGE ?= mooncaker816/micro

DOCKER_TAG ?= $(VERSION)

docker_build:
	docker build \
		-t $(DOCKER_IMAGE):$(DOCKER_TAG) .

docker_push:
	# Tag image as latest
	docker tag $(DOCKER_IMAGE):$(DOCKER_TAG) $(DOCKER_IMAGE):latest

	# Push to DockerHub
	docker push $(DOCKER_IMAGE):$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):latest

output:
	echo Docker Image: $(DOCKER_IMAGE):$(DOCKER_TAG)