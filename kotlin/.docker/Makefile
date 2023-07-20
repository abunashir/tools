export BASE_IMAGE ?= gradle:8.2.1-jdk17-jammy

.PHONY: up
up:
	docker-compose up

.PHONY: down
down:
	docker-compose down

.PHONY: cleanup
cleanup:
	docker-compose down -v

.PHONY: console
console:
	docker-compose run app bash

.PHONY: db-console
db-console:
	docker-compose run db psql -h postgres -d appdb -U postgres

.PHONY: test
test:
	docker-compose run app gradle test

.PHONY: setup
setup: 
	docker-compose build --build-arg NODE_IMAGE=${BASE_IMAGE}
	docker-compose run app gradle build
