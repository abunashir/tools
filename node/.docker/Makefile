export NODE_IMAGE ?= node:18-slim

.PHONY: up
up:
	docker-compose up

.PHONY: down
down:
	docker-compose down

.PHONY: test
test: rspec

.PHONY: ssh
ssh:
	docker-compose run lib bash

.PHONY: rspec
rspec:
	docker-compose run lib bin/rspec ${SPEC_FILE}

.PHONY: setup
setup:
	docker-compose build --build-arg NODE_IMAGE=${NODE_IMAGE}
	docker-compose run lib yarn install
