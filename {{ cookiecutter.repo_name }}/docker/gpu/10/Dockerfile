FROM rapidsai/rapidsai:cuda10.2-base-ubuntu18.04

ENV DEBIAN_FRONTEND=noninteractive
ENV CONDA_DIR=/opt/conda
ENV SHELL=/bin/bash
ENV PATH=$CONDA_DIR/bin:$PATH

RUN APT_UPDATE "apt-get update  -y --fix-missing" && \
    APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    GIT_CLONE="git clone --depth 10" && \
    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \
    $APT_UPDATE && \
    $APT_INSTALL \
        build-essential \
        nano \
        vim \
        apt-utils \
        ca-certificates \
        git \
        curl \
        wget \
        bzip2 \
        zip \
        unzip \
        libssl-dev \
        libncurses5-dev \
        libncursesw5-dev \
        libopenmpi-dev \
        libopenblas-base \
        libopenblas-dev \
        libomp-dev \
        libjpeg-dev \
        libpng-dev \
        libboost-all-dev \
        libsdl2-dev \
        libcurl4-openssl-dev \
        openssh-client \
        openssh-server \
        swig \
        pkg-config \
        g++ \
        zlib1g-dev \
        protobuf-compiler \
        libosmesa6-dev \
        patchelf \
        xvfb \
        && \
        $GIT_CLONE https://github.com/Kitware/CMake ~/cmake && \
        cd ~/cmake && \
        ./bootstrap && \
        make -j"$(nproc)" install && \
    $APT_UPDATE && \
    $APT_INSTALL \
        software-properties-common \
        protobuf-compiler \
        libprotoc-dev \
        libblas-dev \
        libatlas-base-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        openmpi-bin \
        libpng-dev \
        libjpeg-dev \
        libtiff-dev 

RUN conda update -y conda

RUN conda install -c conda-forge -y  nb_conda_kernels

RUN pip install torch

RUN PYTHON_VERSION=cp37  # alternatives: cp36, cp37, cp38 && \
    CUDA_VERSION=cuda102  # alternatives: cuda92, cuda100, cuda101, cuda102 && \
    PLATFORM=linux_x86_64  # alternatives: linux_x86_64 && \
    BASE_URL='https://storage.googleapis.com/jax-releases' && \
    pip install --upgrade $BASE_URL/$CUDA_VERSION/jaxlib-0.1.48-$PYTHON_VERSION-none-$PLATFORM.whl

RUN pip install --upgrade jax  # install jax

# Install requirements
COPY ../../../requirements.txt /tmp/requirements.txt
RUN pip install -r /tmp/requirements.txt

RUN apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*


EXPOSE 8888 6006