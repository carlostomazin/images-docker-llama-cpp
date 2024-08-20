ARG CUDA_IMAGE="12.6.0-devel-ubuntu22.04"
FROM nvidia/cuda:${CUDA_IMAGE}

# Core dependencies linux
RUN apt update && \
    apt upgrade -y && \
    apt install -y \
        python3 \
        python3-pip \
        build-essential \
        gcc \
        wget \
        git && \
    python3 -m pip install --upgrade pip

# Accellerator specific dependencies linux
RUN apt install -y \
        ocl-icd-opencl-dev \
        opencl-headers \
        clinfo \
        libclblast-dev \
        libopenblas-dev \
        && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir -p /etc/OpenCL/vendors && echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd

# Set nvcc architecture
ENV CUDA_DOCKER_ARCH=all
# Enable CUDA
ENV GGML_CUDA=1

RUN git clone https://github.com/ggerganov/llama.cpp
WORKDIR llama.cpp
RUN make -j$(nproc) llama-server

EXPOSE 8080
