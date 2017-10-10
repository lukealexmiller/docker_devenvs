FROM pool1892/docker:caffe_pre

MAINTAINER Luke Miller

ENV CAFFE_ROOT=/root/caffe
WORKDIR $CAFFE_ROOT

ENV CLONE_TAG=master

RUN git clone -b ${CLONE_TAG} --depth 1 https://github.com/BVLC/caffe.git . && \
    cd python && for req in $(cat requirements.txt) pydot; do pip install $req; done && cd .. && \
    git clone https://github.com/NVIDIA/nccl.git && cd nccl && make -j install && cd .. && rm -rf nccl && \
    mkdir build && cd build && \
    cmake -DUSE_CUDNN=1 -DUSE_NCCL=1 -DCUDA_ARCH_NAME=Pascal .. && \
    make -j"$(nproc)"

ENV PYCAFFE_ROOT $CAFFE_ROOT/python
ENV PYTHONPATH $PYCAFFE_ROOT:$PYTHONPATH
ENV PATH $CAFFE_ROOT/build/tools:$PYCAFFE_ROOT:$PATH
RUN echo "$CAFFE_ROOT/build/lib" >> /etc/ld.so.conf.d/caffe.conf && ldconfig

# Expose Ports for Ipython (8888)
EXPOSE  8888

WORKDIR "/root/caffe"
CMD ["/bin/bash"]