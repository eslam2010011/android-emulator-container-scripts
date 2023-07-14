MAKEFILE_PATH := $(abspath $(lastword $(MAKEFILE_LIST)))
CURRENT_DIR := $(abspath $(MAKEFILE_PATH)/..)

.PHONY: build-release run-release develop stop deps audit-fix

all: develop

deps: src/config.js
	@npm install

audit-fix:
	@npm audit fix --force

# This transforms your firebase_config.json into the settings file
# and fixes up you envoy settings to enable token validation.
src/config.js:
	python config_gen.py firebase_config.json

develop:
	./develop.sh

build-release: deps
	docker-compose -f docker/docker-compose.yaml build

run-release: build-release
	docker-compose -f docker/docker-compose.yaml up

stop:
	docker stop emu-dev-grpc-web; pkill -9 npm; pkill -9 js; pkill -9 node; true
