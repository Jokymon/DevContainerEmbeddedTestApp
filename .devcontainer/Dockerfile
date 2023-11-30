# this points to the lts version
FROM ubuntu:latest

# install basics 
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update --fix-missing && apt-get install -y \
      unzip \
      libncurses5 \
      build-essential \
      wget \
      make \
      git \
      cppcheck \
      && rm -rf /var/lib/apt/lists/*

# add the arm toolchain
RUN wget https://developer.arm.com/-/media/Files/downloads/gnu-rm/10.3-2021.10/gcc-arm-none-eabi-10.3-2021.10-x86_64-linux.tar.bz2 \
      -O gcc-arm-none-eabi.tar.bz2 \
      && mkdir /opt/gcc-arm-none-eabi-10.3-2021.10 \
      && tar xjfv gcc-arm-none-eabi.tar.bz2 \
      -C /opt/gcc-arm-none-eabi-10.3-2021.10 --strip-components 1 \
      && rm gcc-arm-none-eabi.tar.bz2 \
      && ln -s /opt/gcc-arm-none-eabi-10.3-2021.10/bin/* /usr/local/bin

# add cmake
ARG CMAKE_VERSION=3.27.7
RUN wget https://github.com/Kitware/CMake/releases/download/v3.27.7/cmake-3.27.7-linux-x86_64.sh \
      -q -O /tmp/cmake-install.sh \
      && chmod u+x /tmp/cmake-install.sh \
      && mkdir /opt/cmake-3.27.7 \
      && ./tmp/cmake-install.sh --skip-license --prefix=/opt/cmake-3.27.7 \
      && rm /tmp/cmake-install.sh \
      && ln -s /opt/cmake-3.27.7/bin/* /usr/local/bin

# add ninja-build
RUN wget https://github.com/ninja-build/ninja/releases/download/v1.11.1/ninja-linux.zip \
      -O /tmp/ninja-build.zip \
      &&  mkdir /opt/ninja-build-1.11.1 \
      && unzip /tmp/ninja-build.zip -d /opt/ninja-build-1.11.1 \
      && rm /tmp/ninja-build.zip \
      && ln -s /opt/ninja-build-1.11.1/* /usr/local/bin

# add a vscode user
RUN useradd -m vscode && chsh -s /bin/bash vscode