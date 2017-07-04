FROM lukemiller/docker:tensorflow

MAINTAINER Luke Miller

RUN apt-get update && apt-get install -y --no-install-recommends \
        git \
        protobuf-compiler \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN pip --no-cache-dir install \
        lxml

# Clone Tensorflow to have /models directory available
RUN cd /root/ && git clone https://github.com/tensorflow/models /root/tensorflow/models

# Compile Protobuf libraries
RUN cd /root/tensorflow/models && protoc object_detection/protos/*.proto --python_out=.

# From tensorflow/models/
ENV PYTHONPATH=/root/tensorflow/models:/root/tensorflow/models/slim

# Set up notebook config
COPY jupyter_notebook_config.py /root/.jupyter/

# Jupyter has issues with being run directly: https://github.com/ipython/ipython/issues/7062
COPY run_jupyter.sh /root/

# Expose Ports for Tensorboard (6006) and Ipython (8888)
EXPOSE 6006 8888

WORKDIR "/root"
CMD ["/bin/bash"]