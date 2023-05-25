# SPDX-License-Identifier: CC-BY-4.0
# Copyright (C) 2023 Wavecon GmbH
# Copyright (C) 2023 CNCF

help:  ## Display this help menu.
	@awk 'BEGIN {FS = ":.*##"; printf "\nUsage:\n  make \033[36m<target>\033[0m\n"} /^[a-zA-Z_0-9-]+:.*?##/ { printf "  \033[36m%-20s\033[0m %s\n", $$1, $$2 } /^##@/ { printf "\n\033[1m%s\033[0m\n", substr($$0, 5) } ' $(MAKEFILE_LIST)
.PHONY: help

deps:
	npm install

serve: deps ## Run documentation preview server
	hugo server \
		--buildDrafts \
		--buildFuture \
		--disableFastRender

build: deps ## Build documentation
	hugo \
		--baseURL $(URL) \
		--gc \
		--minify \
		--enableGitInfo

build-preview: deps ## Build documentation (preview environment)
	hugo \
		--baseURL $(DEPLOY_PRIME_URL) \
		--buildFuture \
		--gc \
		--minify \
		--enableGitInfo
