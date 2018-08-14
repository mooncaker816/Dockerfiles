default: docker_build

# Build Docker image
build: docker_build output

# Build and push Docker image
release: docker_build docker_push output

# run the Docker image as container
run: docker_run

# Image and binary can be overidden with env vars.
DOCKER_IMAGE ?= mooncaker816/centos-asciinema
BINARY ?= centos

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
		-t $(DOCKER_IMAGE):$(DOCKER_TAG) .

docker_push:
	# Tag image as latest
	docker tag $(DOCKER_IMAGE):$(DOCKER_TAG) $(DOCKER_IMAGE):latest

	# Push to DockerHub
	docker push $(DOCKER_IMAGE):$(DOCKER_TAG)
	docker push $(DOCKER_IMAGE):latest

output:
	echo Docker Image: $(DOCKER_IMAGE):$(DOCKER_TAG)

docker_run:
	docker run --rm -ti \
	-v "$HOME/.config/asciinema":/root/.config/asciinema \
	--privileged=true \
	mooncaker816/centos-asciinema