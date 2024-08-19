ARG CUDA_IMAGE="12.6.0-devel-ubuntu22.04"
FROM nvidia/cuda:${CUDA_IMAGE}

# We need to set the host to 0.0.0.0 to allow outside access
ENV HOST=0.0.0.0

RUN apt-get update && apt-get upgrade -y \
    && apt-get install -y git build-essential \
    python3 python3-pip gcc wget git ccache\
    ocl-icd-opencl-dev opencl-headers clinfo \
    libclblast-dev libopenblas-dev \
    && mkdir -p /etc/OpenCL/vendors && echo "libnvidia-opencl.so.1" > /etc/OpenCL/vendors/nvidia.icd

COPY . .

# setting build related env vars
ENV CUDA_DOCKER_ARCH=all
ENV GGML_CUDA=1

# Install depencencies
RUN python3 -m pip install --upgrade pip cmake

# Install llama-cpp (build with cuda)
RUN git clone https://github.com/ggerganov/llama.cpp
WORKDIR llama.cpp
RUN make GGML_CUDA=1 -j 8 llama-server

EXPOSE 8000

# Run the server
# CMD [python3 -m llama_cpp.server]
