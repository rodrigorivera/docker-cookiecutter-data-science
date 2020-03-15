FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu18.04
ADD environment.yml /tmp/environment.yml
ENV LANG C.UTF-8
ENV PATH /opt/conda/bin:$PATH
RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \

    rm -rf /var/lib/apt/lists/* \
           /etc/apt/sources.list.d/cuda.list \
           /etc/apt/sources.list.d/nvidia-ml.list && \

    apt-get update && \

# ==================================================================
# tools
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        build-essential \
        apt-utils \
        ca-certificates \
        wget \
        git \
        vim \
        curl \
        wget \
        bzip2 \
        ca-certificates \
        locales \
        fonts-liberation \
        inkscape \
        jed \
        libsm6 \
        libxext-dev \
        libxrender1 \
        lmodern \
        netcat \
        pandoc \
        texlive-fonts-recommended \
        texlive-generic-recommended \
        texlive-latex-base \
        texlive-latex-extra \
        texlive-xetex \
        ffmpeg \
        graphviz\
        nano \
        vim \
        emacs \
        htop \
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


# ==================================================================
# debian
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        software-properties-common \
        && \
    add-apt-repository ppa:deadsnakes/ppa && \
    apt-get update && \

    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        libatlas-base-dev \
        graphviz \
        && \

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
        libtiff-dev \
        && \

# ==================================================================
# R
# ------------------------------------------------------------------

    DEBIAN_FRONTEND=noninteractive apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9 \
        && \
        add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/' \
        && \
        apt update \
        && \
    DEBIAN_FRONTEND=noninteractive $APT_INSTALL \
        r-base \
        r-base-dev \
        && \
# ==================================================================
# python
# ------------------------------------------------------------------
    wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
    -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh
ENV PATH /opt/conda/bin:$PATH

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    conda env create -f /tmp/environment.yml && \
    # Pull the environment name out of the environment.yml
    echo "conda activate $(head -1 /tmp/environment.yml | cut -d' ' -f2)" >> ~/.bashrc
ENV PATH="/opt/conda/envs/$(head -1 /tmp/environment.yml | cut -d' ' -f2)/bin:${PATH}"

RUN APT_INSTALL="apt-get install -y --no-install-recommends" && \
    PIP_INSTALL="python -m pip --no-cache-dir install --upgrade" && \
    CONDA_INSTALL="conda install -y" && \
    GIT_CLONE="git clone --depth 10" && \

    # create the environment
    #conda update -n $(head -1 /tmp/environment.yml | cut -d' ' -f2) -y && \
    conda env update -n $(head -1 /tmp/environment.yml | cut -d' ' -f2) --file /tmp/environment.yml && \

# ==================================================================
# config & cleanup
# ------------------------------------------------------------------

    ldconfig && \
    apt-get clean && \
    apt-get autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* ~/*


EXPOSE 8888 6006
