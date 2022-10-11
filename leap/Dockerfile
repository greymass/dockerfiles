FROM ubuntu:22.04 as build-stage

# install build deps
RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
        git \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# setup build time variables
ARG LEAP_VERSION=v3.1.2
ARG LEAP_REPOSITORY=https://github.com/AntelopeIO/leap.git

# build and install leap
WORKDIR /tmp
RUN git clone --branch $LEAP_VERSION --depth 1 $LEAP_REPOSITORY . \
  && git submodule update --init --recursive
RUN ./scripts/install_deps.sh
RUN ./scripts/pinned_build.sh /tmp/leap /leap $(nproc)

# runtime image
FROM ubuntu:22.04

# runtime deps
RUN apt-get update && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
  libusb-1.0-0 \
  && rm -rf /var/lib/apt/lists/*

# copy leap install
COPY --from=build-stage /leap /leap

# setup runtime env
ARG LEAP_VERSION=v3.1.2
ENV LEAP_VERSION=$LEAP_VERSION
ENV PATH="/leap/bin:${PATH}"
