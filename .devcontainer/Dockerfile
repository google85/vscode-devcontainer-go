ARG GO_VERSION=1.15

FROM golang:${GO_VERSION}-buster

ENV TZ "Europe/Bucharest"
ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=1000

# update + install missing deps
RUN apt-get update && apt-get install -y \
    sudo \
    --no-install-recommends

# Create non-root user
RUN groupadd --gid $USER_GID $USERNAME && \
    useradd --uid $USER_UID --gid $USER_GID -c "VsCode user" -s /bin/bash  -m $USERNAME

# add to root
RUN mkdir -p /etc/sudoers.d && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

#RUN mkdir -p /go/pkg && chmod 0755 /go/pkg

# optional after-setup script
COPY --chown=1000:1000 ./after-setup.sh /usr/bin/after-setup.sh
RUN chmod +x "/usr/bin/after-setup.sh"

WORKDIR /usr/src/app

# Optional
#COPY ./go.mod ./go.sum ./
#RUN go mod download

# gopls v0.9.5 is latest compatible with Go v1.15
RUN GO111MODULE=on  env GOPATH=/go  go get golang.org/x/tools/gopls@v0.9.5

RUN GO111MODULE=on  env GOPATH=/go  go get github.com/rogpeppe/godef

RUN GO111MODULE=on  env GOPATH=/go  go get github.com/ramya-rao-a/go-outline

# Required for Go extension (failed because io/fs not found)
#   RUN GO111MODULE=on  go get -u golang.org/x/tools/cmd/goimports


# Optional, only if you need hot reload (requires io/fs)
#RUN GO111MODULE=on  env GOPATH=/go   go get github.com/cosmtrek/air

# fix permissions for non-root go install
RUN chown -R 1000:1000 /go

# non-root user, for git commit purposes
USER ${USERNAME}
