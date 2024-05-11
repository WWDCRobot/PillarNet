FROM pytorch/pytorch:1.12.1-cuda11.3-cudnn8-devel

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

ENV PATH /opt/conda/bin:$PATH

# common python package 
RUN /opt/conda/bin/pip install --no-cache-dir fire easydict pyyaml llvmlite numba terminaltables shapely --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir opencv-python tqdm SharedArray av2 kornia pyquaternion open3d addict --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir onnx onnxsim onnx-simplifier onnxruntime-gpu onnxoptimizer --timeout=100

# CenterPoint
RUN /opt/conda/bin/pip install --no-cache-dir protobuf pybind11 pytest-runner pycocotools imagecorruptions objgraph --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir cachetools descartes motmetrics pandas scikit-learn requests nuscenes-devkit --timeout=100
RUN /opt/conda/bin/pip install --no-cache-dir torch_scatter spconv-cu113 --timeout=100


WORKDIR /workspace


CMD ["\bin\bash"]

                                                                                            
