export TEST_FILE ?= tests
export IMAGE ?= python:3.10-bullseye
NORMALIZE_TEST_FILE = $(subst ../,, $(TEST_FILE))

.PHONY: up
up:
	docker-compose up

.PHONY: down
down:
	docker-compose down

.PHONY: ssh
ssh:
	docker-compose run app bash

.PHONY: fmt
fmt:
	docker-compose run app black /app/src

.PHONY: test
test:
	docker-compose run app pytest ${NORMALIZE_TEST_FILE}

.PHONY: setup
setup:
	docker-compose build --build-arg BASE_IMAGE=${IMAGE}
	docker-compose run app poetry install
