FROM python:3.8.2-buster

ARG snetd_version

ENV SINGNET_REPOS=/opt/singnet

RUN mkdir -p ${SINGNET_REPOS}

RUN apt-get update; \
    apt-get install -y \
    python3.7-dev nano zsh libudev-dev libusb-1.0-0-dev

RUN yes | sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

RUN cd /tmp; \
    curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh; \
    bash nodesource_setup.sh; \
    apt-get install -y nodejs; \
    rm -f nodesource_setup.sh

# Protobuf tools
RUN apt-get install -y apt install protobuf-compiler; \
    npm install -g ts-protoc-gen

RUN pip install snet-cli

RUN SNETD_GIT_VERSION=`curl -s https://api.github.com/repos/singnet/snet-daemon/releases/latest | grep -oP '"tag_name": "\K(.*)(?=")' || echo "v3.1.6"`; \
    SNETD_VERSION=${snetd_version:-${SNETD_GIT_VERSION}}; \
    cd /tmp; \
    wget https://github.com/singnet/snet-daemon/releases/download/${SNETD_VERSION}/snet-daemon-${SNETD_VERSION}-linux-amd64.tar.gz; \
    tar -xvf snet-daemon-${SNETD_VERSION}-linux-amd64.tar.gz; \
    mv snet-daemon-${SNETD_VERSION}-linux-amd64/snetd /usr/bin/snetd; \
    rm -rf snet-daemon-*

WORKDIR ${SINGNET_REPOS}