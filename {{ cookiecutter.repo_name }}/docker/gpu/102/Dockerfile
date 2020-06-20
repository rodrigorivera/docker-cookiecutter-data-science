FROM rapidsai/rapidsai:cuda10.2-base-ubuntu18.04
ENV CONDA_DIR=/opt/conda
ENV SHELL=/bin/bash
ENV PATH=$CONDA_DIR/bin:$PATH
RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \
    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        build-essential \
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
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        protobuf-compiler \
        libprotoc-dev \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libblas-dev \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        libgflags-dev \
        libgoogle-glog-dev \
        libhdf5-serial-dev \
        libleveldb-dev \
        liblmdb-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        openmpi-bin \
        libpng-dev \
        libjpeg-dev \
        libtiff-dev 

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \   
    conda install -y pytorch torchvision cudatoolkit=10.2 -c pytorch \
    && \
    conda install -y -c anaconda tensorflow \
    && \
    conda install -y -c conda-forge jax \
    && \
    conda install -y pandas\
    && \
    conda install -y scikit-learn\
    && \
    conda install -y numpy\
    && \
    conda install -y scipy\
    && \
    conda install -y matplotlib\
    && \
    conda install -y typing\
    && \
    conda install -y jupyter\
    && \
    conda install -y dask \ 
    && \
    conda install -y Cython \ 
    && \
    conda install -y ipykernel \
    && \
    conda clean --all -f -y

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \
    conda install -y -c conda-forge nb_conda_kernels


RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*


EXPOSE 8888 6006