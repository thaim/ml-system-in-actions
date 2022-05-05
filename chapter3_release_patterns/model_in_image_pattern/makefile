DOCKER_REPOSITORY := shibui/ml-system-in-actions

ABSOLUTE_PATH := $(shell pwd)

DOCKERFILE := Dockerfile
IMAGE_VERSION := 0.0.1

MODEL_IN_IMAGE_PATTERN := model_in_image_pattern
MODEL_IN_IMAGE_PATTERN_PORT := 8000

.PHONY: build
build:
	docker build \
		-t $(DOCKER_REPOSITORY):$(MODEL_IN_IMAGE_PATTERN)_$(IMAGE_VERSION) \
		-f $(DOCKERFILE) \
		.
	 
.PHONY: push
push:
	docker push $(DOCKER_REPOSITORY):$(MODEL_IN_IMAGE_PATTERN)_$(IMAGE_VERSION)

.PHONY: build_all
build_all: build 

.PHONY: push_all
push_all: push

.PHONY: deploy
deploy:
	kubectl apply -f manifests/namespace.yml
	kubectl apply -f manifests/deployment.yml

.PHONY: delete
delete:
	kubectl delete ns model-in-image

.PHONY: port_forward
port_forward:
	kubectl -n model-in-image port-forward deployment.apps/model-in-image 8000:8000

.PHONY: predict
predict:
	curl -X POST \
		 -H "Content-Type: application/json" \
		 -d '{"data": [[1.0, 2.0, 3.0, 4.0]]}' \
		 localhost:8000/predict
