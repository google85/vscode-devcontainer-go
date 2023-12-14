all: build

BUILD_FLAGS = -ldflags "-s -w"
OUTPUT_BIN = ../bin/main

build:
	@echo 'Building..'
	@go env -w GO111MODULE=on
	@env GOOS=linux GOARCH=amd64 CGO_ENABLED=0 \
	go build -o ${OUTPUT_BIN} ${BUILD_FLAGS} main.go
#	GOPATH=${GOPATH}:${PWD}/src/include  \

clean:
	@echo 'Cleaning..'
	rm -rf ../bin

check_install:
# 	install if not exists
	@which swagger || GO111MODULE=off go get -u github.com/go-swagger/go-swagger/cmd/swagger

swagger: check_install
	@GO111MODULE=off swagger generate spec -o ./swagger.yaml --scan-models
