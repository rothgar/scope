.PHONY: all vet lint build test clean

DIRS=$(shell find . -maxdepth 2 -name *.go | xargs -n1 dirname | sort -u)
BUILD_IN_CONTAINER=true
RM=--rm

all: build test vet lint

ifeq ($(BUILD_IN_CONTAINER),true)
build:
	$(SUDO) docker run -ti $(RM) -v $(shell pwd)/../:/go/src/github.com/weaveworks/scope -e GOARCH -e GOOS \
		weaveworks/scope-backend-build -C fixprobe build
else
build:
	go build -ldflags "-extldflags \"-static\"" -tags netgo -o fixprobe/fixprobe ./$(@D)
endif

test:
	go test ./...

clean:
	go clean ./...

vet:
	go vet ./...

lint:
	golint .

