default: docker_build

# Build Docker image
build: docker_build output

# Build and push Docker image
release: docker_build docker_push output

# Image and binary can be overidden with env vars.
DOCKER_IMAGE ?= mooncaker816/caddy-hugo
BINARY ?= caddy

CODE_VERSION = $(strip $(shell cat VERSION))
# Get the latest commit.
GIT_COMMIT = $(strip $(shell git rev-parse --short HEAD))
DOCKER_TAG = $(CODE_VERSION)-$(GIT_COMMIT)


docker_build:
	docker build \
		--build-arg VCS_REF=$(GIT_COMMIT) \
		--build-arg BUILD_DATE=`date -u +"%Y-%m-%dT%H:%M:%SZ"` \
		--build-arg VERSION=$(CODE_VERSION) \
		--build-arg VCS_URL=`git config --get remote.origin.url` \
		--build-arg PLUGINS="http.git,http.hugo" \
		--build-arg HUGO_VERSION="0.52" \
		--build-arg CADDY_VERSION="0.11.1" \
		-t $(DOCKER_IMAGE):$(DOCKER_TAG) .

docker_push:
	# Tag image as latest
	docker tag $(DOCKER_IMAGE):$(DOCKER_TAG) $(DOCKER_IMAGE):latest

	# Push to DockerHub
	docker push $(DOCKER_IMAGE):$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):latest

output:
	echo Docker Image: $(DOCKER_IMAGE):$(DOCKER_TAG)