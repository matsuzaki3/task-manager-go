.PHONY: help

VERSION=$(shell git rev-parse --verify HEAD)

help: ## help
	@awk 'BEGIN {FS = ":.*?## "} /^[a-zA-Z_-]+:.*?## / {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}' $(MAKEFILE_LIST)

GOBUILD="GOOS=darwin GOARCH=amd64 go build -o ."
GOBUILD_REMOTE_DEBUG="GOOS=darwin GOARCH=amd64 go build -gcflags 'all=-N -l' -o ."

init: ## initialize
	go mod download

go-lint:  ## golangのlinterを実行
	golangci-lint run ./...

go-lint-fmt:  ## golangのlinterとコードフォーマッタも実行
	golangci-lint run --fix ./...

specify-go-lint:  ## linterを指定して実行 `ex: make specify-go-lint LINT=staticcheck`
	golangci-lint run --disable-all -E ${LINT} ./src...

test: ## go test
	WEB_ENV=test go test ./... -v -cover +local -tags=tests -count=1

check: ## check
	make go-lint && make test

build: ## build
	eval ${GOBUILD} main.go

build-remote-debug: ## build-remote-debug
	eval ${GOBUILD_REMOTE_DEBUG} main.go
