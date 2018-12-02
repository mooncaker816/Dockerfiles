default: docker_build

# Build Docker image
build: docker_build output

# Build and push Docker image
release: docker_build docker_push output

VERSION = latest

# Image and binary can be overidden with env vars.
DOCKER_IMAGE ?= mooncaker816/goenv

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

run:
	docker run -it --rm \
	-v ${HOME}/goenv/linux/go:/go \
	-w /go \
	--cap-add=SYS_PTRACE --security-opt seccomp=unconfined \
	mooncaker816/goenv /bin/sh