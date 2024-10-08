ARG UBUNTU_VERSION=22.04
ARG CUDA_VERSION=12.4.0
FROM nvidia/cuda:${CUDA_VERSION}-devel-ubuntu${UBUNTU_VERSION}

# Core dependencies linux
RUN apt update && \
    apt upgrade -y && \
    apt install -y \
        python3 \
        python3-pip \
        build-essential \
        git && \
    pip3 install --upgrade pip

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

# Install llama-cpp-python (build with cuda)
RUN CMAKE_ARGS="-DGGML_CUDA=on" pip3 install llama_cpp_python[server] --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cu124

EXPOSE 8080
