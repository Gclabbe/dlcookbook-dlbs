FROM nvidia/cuda:9.0-cudnn7-devel-ubuntu16.04
MAINTAINER sergey.serebryakov@hpe.com

RUN apt-get update && apt-get install -y --no-install-recommends \
        numactl \
        build-essential \
        cmake \
        git \
        libgflags-dev \
        libgoogle-glog-dev \
        libgtest-dev \
        libiomp-dev \
        libleveldb-dev \
        liblmdb-dev \
        libopencv-dev \
        libopenmpi-dev \
        libprotobuf-dev \
        libsnappy-dev \
        protobuf-compiler \
        python-dev \
        python-numpy \
        python-pip \
        python-pydot \
        python-setuptools \
        python-scipy \
        wget \
        ca-certificates && \
    rm -rf /var/lib/apt/lists/*

RUN pip install --no-cache-dir --upgrade pip setuptools wheel && \
    pip install --no-cache-dir \
        flask \
        future \
        graphviz \
        hypothesis \
        jupyter \
        matplotlib \
        numpy \
        protobuf \
        pydot \
        python-nvd3 \
        pyyaml \
        requests \
        scikit-image \
        scipy \
        setuptools \
        six \
        tornado

# This is where Caffe will be installed
ENV CAFFE2_ROOT=/opt/caffe2

ARG version=master
RUN cd / && \
    git clone --recursive https://github.com/caffe2/caffe2.git && cd caffe2 && \
    git reset --hard $version && git submodule update && \
    mkdir ./build && cd ./build && \
    cmake -DUSE_CUDA=ON\
          -DUSE_NCCL=ON\
          -DBUILD_TEST=OFF\
          -DUSE_OPENMP=ON\
          -DCMAKE_INSTALL_PREFIX=$CAFFE2_ROOT .. && \
    make -j"$(nproc)" && make install && \
    cd ../../ && rm -rf ./caffe2


ENV PYTHONPATH $CAFFE2_ROOT:$PYTHONPATH
ENV PATH $CAFFE2_ROOT/binaries:$PATH
RUN echo "$CAFFE2_ROOT/lib" >> /etc/ld.so.conf.d/caffe2.conf && \
    ldconfig

WORKDIR /workspace
