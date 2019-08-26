default: docker_build

# Build Docker image
build: docker_build output

# Build and push Docker image
release: docker_build docker_push output

# Image and binary can be overidden with env vars.
DOCKER_IMAGE ?= mooncaker816/caddy-fb


docker_build:
	docker build \
		--build-arg PLUGINS="http.git,http.ipfilter,http.prometheus" \
		-t $(DOCKER_IMAGE):latest .

docker_push:

	# Push to DockerHub
	docker push $(DOCKER_IMAGE):latest

output:
	echo Docker Image: $(DOCKER_IMAGE):latest