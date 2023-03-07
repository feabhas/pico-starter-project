
FROM ubuntu:22.04

ENV REFRESHED_AT 2023-03-07

# [Optional] Uncomment this section to install additional packages.
RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    wget \
    git \
    build-essential \
    cmake \
    ninja-build=1.10.1-1 \
    cppcheck \
    python3 \
    python3-pip \
    clang-tidy \
    clang-format \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

RUN apt-get update && export DEBIAN_FRONTEND=noninteractive \
    && apt-get -y install --no-install-recommends \
    build-essential \
    cmake \
    ninja-build=1.10.1-1 \
    cppcheck \
    valgrind \
    clang-tidy \
    clang-format \
    && apt-get autoremove -y && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Installing Arm GCC

# WORKDIR /dependencies
# COPY gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2 .
# RUN wget --no-check-certificate --progress=dot:giga https://developer.arm.com/-/media/Files/downloads/gnu-rm/10-2020q4/gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2

# WORKDIR /opt
# RUN tar xf /dependencies/gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2 \
#     && rm -rf gcc-arm-none-eabi-10-2020-q4-major-x86_64/share/doc

WORKDIR /opt/
RUN cd /opt/ && \
    wget -q https://developer.arm.com/-/media/Files/downloads/gnu-rm/10-2020q4/gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2 && \
    echo "8312c4c91799885f222f663fc81f9a31  gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2" >> MD5SUM && \
    md5sum -c MD5SUM && \
    rm MD5SUM && \
    tar -xf gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2 && \
    rm gcc-arm-none-eabi-10-2020-q4-major-x86_64-linux.tar.bz2

ENV PATH=/opt/gcc-arm-none-eabi-10-2020-q4-major/bin:$PATH

#install SDK
WORKDIR /workspaces/
RUN cd /workspaces/ && \
    git clone --depth=1 --recursive https://github.com/raspberrypi/pico-sdk.git

ENV PICO_SDK_PATH="/workspaces/pico-sdk/"



RUN adduser pico

WORKDIR /home/pico




