FROM ubuntu:22.04 as build-stage

# install build deps
RUN apt-get update \
    && DEBIAN_FRONTEND="noninteractive" apt-get install -y --no-install-recommends \
        git \
        ca-certificates \
    && rm -rf /var/lib/apt/lists/*

# setup build time variables
ARG CHRONICLE_VERSION=v2.2
ARG CHRONICLE_REPOSITORY=https://github.com/EOSChronicleProject/eos-chronicle.git

# build and install chronicle
WORKDIR /tmp
RUN git clone --branch $CHRONICLE_VERSION --depth 1 $CHRONICLE_REPOSITORY . \
  && git submodule update --init --recursive
RUN ./pinned_build/install_deps.sh
RUN ./pinned_build/chronicle_pinned_build.sh /tmp/chronicle /chronicle $(nproc)

# runtime image
FROM ubuntu:22.04

# copy chronicle install
COPY --from=build-stage /chronicle /chronicle
RUN ln -s /chronicle/chronicle-receiver /usr/local/bin/chronicle-receiver

# setup runtime env
ARG CHRONICLE_VERSION=v2.2
ENV CHRONICLE_VERSION=$CHRONICLE_VERSION
ENV PATH="/usr/local/bin:${PATH}"
