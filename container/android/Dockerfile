FROM bitriseio/android-ndk:v2021_05_29-08_17-b2417

#ARG PYTHON_VERSION=3.12.2

#ENV PYTHON_VERSION=${PYTHON_VERSION}

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
 && DEBIAN_FRONTEND=noninteractive apt -y install \
    curl dirmngr apt-transport-https lsb-release ca-certificates

# Installs glibc.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y glibc2.28

# Installs node.
RUN curl -sSL https://deb.nodesource.com/setup_20.x | bash - && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y nodejs

# Installs yarn.
RUN curl -sSL https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    DEBIAN_FRONTEND=noninteractive apt-get update && apt-get install -y yarn

# Installs envsubst.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y gettext-base

# Misc build dependencies.
RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
 && DEBIAN_FRONTEND=noninteractive apt-get install -y git rsync

# We need latest jq from debian buster for date support.
RUN ARCH="$(dpkg --print-architecture)" && \
    curl -sSOL http://http.us.debian.org/debian/pool/main/libo/libonig/libonig5_6.9.1-1_$ARCH.deb && \
    dpkg -i libonig*.deb && \
    curl -sSOL http://http.us.debian.org/debian/pool/main/j/jq/libjq1_1.5+dfsg-2+b1_$ARCH.deb && \
    dpkg -i libjq*.deb && \
    curl -sSOL http://http.us.debian.org/debian/pool/main/j/jq/jq_1.5+dfsg-2+b1_$ARCH.deb && \
    dpkg -i jq*.deb && rm *.deb

RUN DEBIAN_FRONTEND=noninteractive apt -y autoremove

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
    && DEBIAN_FRONTEND=noninteractive apt-get install -y make vim nano

RUN update-alternatives --remove-all gcc || true; update-alternatives --remove-all g++ || true

RUN add-apt-repository ppa:ubuntu-toolchain-r/test && \
    DEBIAN_FRONTEND=noninteractive apt-get update && \
    DEBIAN_FRONTEND=noninteractive apt-get install gcc-9 g++-9 gcc-9-multilib g++-9-multilib -y

RUN DEBIAN_FRONTEND=noninteractive apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y libbz2-dev

RUN update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 10 && \
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 10 && \
    update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30 && \
    update-alternatives --set cc /usr/bin/gcc && \
    update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30 && \
    update-alternatives --set c++ /usr/bin/g++

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
 && DEBIAN_FRONTEND=noninteractive apt-get -y install libc6-dev-i386
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y
RUN . ~/.cargo/env && rustup target add x86_64-linux-android i686-linux-android aarch64-linux-android armv7-linux-androideabi

ENV NDK=/opt/android-ndk
ENV NDK_BIN_ROOT=$NDK/toolchains/llvm/prebuilt/linux-x86_64/bin
ENV ANDROID_ARCH=aarch64

RUN DEBIAN_FRONTEND=noninteractive apt-get -y update \
 && DEBIAN_FRONTEND=noninteractive apt-get -y install quilt

RUN git config --global --add safe.directory /vscode \
    && git config --global --add safe.directory /vscode/code-server \
    && git config --global --add safe.directory /vscode/code-server/lib/vscode \
    && git config --global --add safe.directory /vscode/node-src

ENTRYPOINT /bin/bash
