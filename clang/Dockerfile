FROM ubuntu:24.04

ARG LLVM_VERSION

LABEL org.opencontainers.image.source=https://github.com/k3DW/docker-images
LABEL org.opencontainers.image.description="Clang ${LLVM_VERSION} with libc++ built from source"

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    ninja-build \
    python3 \
    git \
    ca-certificates \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --depth 1 --branch llvmorg-${LLVM_VERSION} \
    https://github.com/llvm/llvm-project.git /llvm-project

RUN cmake -S /llvm-project/llvm -B /llvm-build \
    -G Ninja \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=/usr/local \
    -DLLVM_ENABLE_PROJECTS="clang;lld" \
    -DLLVM_ENABLE_RUNTIMES="libcxx;libcxxabi;libunwind" \
    -DLLVM_TARGETS_TO_BUILD=X86 \
    -DLLVM_INCLUDE_TESTS=OFF \
    -DLLVM_INCLUDE_EXAMPLES=OFF \
    -DLLVM_INCLUDE_BENCHMARKS=OFF \
    -DCLANG_INCLUDE_TESTS=OFF \
  && cmake --build /llvm-build \
  && cmake --install /llvm-build \
  && rm -rf /llvm-project /llvm-build

RUN ldconfig
