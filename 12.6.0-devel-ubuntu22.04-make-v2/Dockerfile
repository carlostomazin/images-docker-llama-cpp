ARG UBUNTU_VERSION=22.04
# This needs to generally match the container host's environment.
ARG CUDA_VERSION=12.6.0
# Target the CUDA build image
ARG BASE_CUDA_DEV_CONTAINER=nvidia/cuda:${CUDA_VERSION}-devel-ubuntu${UBUNTU_VERSION}
# Target the CUDA runtime image
ARG BASE_CUDA_RUN_CONTAINER=nvidia/cuda:${CUDA_VERSION}-runtime-ubuntu${UBUNTU_VERSION}

# IMAGE 1 --------------------------------------------------------------------------------------
FROM ${BASE_CUDA_DEV_CONTAINER} AS build

RUN apt update && \
    apt install -y \
        build-essential \
        git \
        libcurl4-openssl-dev

# Set nvcc architecture
ENV CUDA_DOCKER_ARCH=all
# Enable CUDA
ENV GGML_CUDA=1

RUN git clone https://github.com/ggerganov/llama.cpp
WORKDIR /llama.cpp
RUN make -j$(nproc) llama-server

# IMAGE 2 --------------------------------------------------------------------------------------
FROM ${BASE_CUDA_RUN_CONTAINER} AS runtime

RUN apt update && \
    apt install -y \
        python3 \
        python3-pip \
        libcurl4-openssl-dev \
        libgomp1 \
        ocl-icd-opencl-dev \
        opencl-headers \
        clinfo \
        libclblast-dev \
        libopenblas-dev \
        curl && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /etc/OpenCL/vendors && echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd

COPY --from=build /llama.cpp/llama-server /llama-server

HEALTHCHECK CMD [ "curl", "-f", "http://localhost:8080/health" ]

EXPOSE 8080
