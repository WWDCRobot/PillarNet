FROM nvidia/cuda:11.8.0-cudnn8-devel-ubuntu22.04

ARG PYTHON_VERSION=3.10
ARG CUDA_VERSION=11.8
ARG CUDA_CHANNEL=nvidia
ARG PYTORCH_VERSION=2.2.2
ARG TORCHVISION_VERSION=0.17.2
ARG TORCHAUDIO_VERSION=2.2.2

ENV TZ=Asia/Shanghai

# /bin/bash 
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

# install sshd 
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y openssh-server 
RUN service ssh start

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    tzdata build-essential ca-certificates ccache libjpeg-dev libpng-dev && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    libboost-all-dev libflann-dev libyaml-cpp-dev libgflags-dev \
    git zip unzip vim cmake libssl-dev libcairo2-dev lsb-release libgoogle-glog-dev libatlas-base-dev \
    build-essential dos2unix pkg-config libeigen3-dev libapr1-dev autoconf automake libtool curl htop tree gdb \
    libsparsehash-dev wget ffmpeg libsm6 libxext6 ninja-build libglib2.0-0 libxrender-dev && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# conda 
RUN wget -O /root/miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-py310_24.3.0-0-Linux-x86_64.sh

# Manually invoke bash on miniconda script per https://github.com/conda/conda/issues/10431
RUN chmod +x /root/miniconda.sh && \
    bash /root/miniconda.sh -b -p /opt/conda && \
    rm /root/miniconda.sh && \
    /opt/conda/bin/conda install -y python=${PYTHON_VERSION} cmake conda-build numpy ipython && \
    /opt/conda/bin/conda clean -ya

ENV PATH /opt/conda/bin:$PATH
RUN /opt/conda/bin/conda shell.bash hook >> ~/.bashrc
RUN /opt/conda/bin/conda config --set show_channel_urls yes


# pytorch 
RUN /opt/conda/bin/pip install --no-cache-dir torch==${PYTORCH_VERSION} torchvision==${TORCHVISION_VERSION} torchaudio==${TORCHAUDIO_VERSION} --index-url https://download.pytorch.org/whl/cu118 && \
    /opt/conda/bin/conda clean -ya


# common python package 
RUN /opt/conda/bin/pip install --no-cache-dir fire easydict pyyaml llvmlite numba terminaltables shapely --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir opencv-python tqdm SharedArray av2 kornia pyquaternion open3d addict --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir onnx onnxsim onnx-simplifier onnxruntime-gpu onnxoptimizer --timeout=100


# CenterPoint
RUN /opt/conda/bin/pip install --no-cache-dir protobuf pybind11 pytest-runner pycocotools imagecorruptions objgraph --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir cachetools descartes motmetrics pandas scikit-learn requests nuscenes-devkit --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir torch_scatter spconv-cu118 --timeout=100


WORKDIR /workspace


CMD ["\bin\bash"]

                                                                                            
