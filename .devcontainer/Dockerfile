ARG GO_VERSION=1.19.0

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
    useradd --uid $USER_UID --gid $USER_GID -m $USERNAME

# add to root
RUN mkdir -p /etc/sudoers.d && \
    echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# optional after-setup script
COPY --chown=1000:1000 ./after-setup.sh /usr/bin/after-setup.sh
RUN chmod +x "/usr/bin/after-setup.sh"

WORKDIR /usr/src/app

# Optional
#COPY ./go.mod ./go.sum ./
#RUN go mod download

# Required for Go extension
RUN go install golang.org/x/tools/gopls@latest

# Optional, only if you need hot reload
RUN go install github.com/cosmtrek/air@latest

# non-root user, for git commit purposes
USER ${USERNAME}
