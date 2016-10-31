.PHONY: clean build run stop inspect

IMAGE_NAME = hypebeast/alpine-flask
CONTAINER_NAME = alpine-flask
TAG = 0.1.0-python27

build:
	docker build -t $(IMAGE_NAME):$(TAG) .

run:
	docker run --rm -it --name $(CONTAINER_NAME) -p 5000:80 $(IMAGE_NAME):$(TAG)

inspect:
	docker exec -it $(CONTAINER_NAME) /bin/bash

stop:
	docker stop $(CONTAINER_NAME)

clean:
	docker ps -a | grep '$(CONTAINER_NAME)' | awk '{print $$1}' | xargs docker rm \
	docker images | grep '$(IMAGE_NAME)' | awk '{print $$3}' | xargs docker rmi
