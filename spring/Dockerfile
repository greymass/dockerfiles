FROM ubuntu:22.04 as build-stage

# install build deps
RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
        git \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# setup build time variables
ARG SPRING_VERSION=v1.0.1
ARG SPRING_REPOSITORY=https://github.com/AntelopeIO/spring.git

# build and install spring
WORKDIR /tmp
RUN git clone --branch $SPRING_VERSION --depth 1 $SPRING_REPOSITORY . \
  && git submodule update --init --recursive
RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
        build-essential \
        cmake \
        libcurl4-openssl-dev \
        libgmp-dev \
        llvm-11-dev \
        python3-numpy \
        file \
        zlib1g-dev \
    && rm -rf /var/lib/apt/lists/*
RUN mkdir -p build \
    && cd build \
    ## on Ubuntu 22, the default gcc version is 11, using the default compiler is fine
    && cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_PREFIX_PATH=/usr/lib/llvm-11 .. \
    && make -j "$(nproc)" \
    && cd /tmp

# runtime image
FROM ubuntu:22.04

# runtime deps
RUN apt-get update && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
  libusb-1.0-0 \
  && rm -rf /var/lib/apt/lists/*

# copy spring install
COPY --from=build-stage /tmp/build /spring

# setup runtime env
ARG SPRING_VERSION=v5.0.0
ENV SPRING_VERSION=$SPRING_VERSION
ENV PATH="/spring/bin:${PATH}"
