# Copyright 2018 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

PROJECT ?= rules-k8s
IMG = gcr.io/$(PROJECT)/gcloud-bazel
OLD ?= 3.5.0
NEW ?= 4.1.0
TAG := $(shell date +v%Y%m%d)-$(shell git describe --tags --always --dirty)

build:
	docker build --pull -t $(IMG):latest-$(USER) --build-arg OLD_VERSION=$(OLD) --build-arg NEW_VERSION=$(NEW) .

push:
	gcloud builds submit \
		--config=cloudbuild.yaml \
		--substitutions=_IMG=$(IMG),_USER=$(USER),SHORT_SHA=$(TAG),_NEW_VERSION=$(NEW),_OLD_VERSION=$(OLD) \
		--project=$(PROJECT) \
		.

pull: push
	docker pull $(IMG):latest
	docker pull $(IMG):latest-$(USER)
	docker pull $(IMG):$(TAG)

all: pull

.PHONY: all build push
