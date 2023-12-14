all: build

BUILD_FLAGS = -ldflags "-s -w"
OUTPUT_BIN = ../bin/main

build:
	@echo 'Building..'
	@go env -w GO111MODULE=on
	@env CGO_ENABLED=0 go build -o ${OUTPUT_BIN} ${BUILD_FLAGS} main.go

clean:
	@echo 'Cleaning..'
	rm -rf ../bin
