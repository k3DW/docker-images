FROM ubuntu:24.04

ARG GCC_VERSION

LABEL org.opencontainers.image.source=https://github.com/k3DW/docker-images
LABEL org.opencontainers.image.description="GCC ${GCC_VERSION} built from source"

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    ninja-build \
    python3 \
    git \
    ca-certificates \
    libgmp-dev \
    libmpfr-dev \
    libmpc-dev \
    zlib1g-dev \
    flex \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 --branch releases/gcc-${GCC_VERSION} \
    https://github.com/gcc-mirror/gcc.git /gcc-source

RUN mkdir /gcc-build \
  && cd /gcc-build \
  && /gcc-source/configure \
    --prefix=/usr/local \
    --enable-languages=c,c++ \
    --disable-multilib \
    --disable-bootstrap \
    --with-system-zlib \
  && make -j$(nproc) \
  && make install \
  && rm -rf /gcc-source /gcc-build

RUN find /usr/local/lib -maxdepth 2 -name "libstdc++.so.6" -exec dirname {} \; > /etc/ld.so.conf.d/gcc.conf \
  && ldconfig
