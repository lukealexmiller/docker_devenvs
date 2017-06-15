FROM nvidia/cuda:8.0-cudnn5-devel

MAINTAINER Luke Miller

# Faster R-CNN has OpenCV dependency
RUN apt-get update && apt-get install -y --no-install-recommends \
        python-opencv \
        python-tk \
        libopencv-dev \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Install other useful Python packages using pip
RUN pip --no-cache-dir install --upgrade ipython && \
    pip --no-cache-dir install \
        easydict \
        Cython \
        pyyaml \
        scikit-image \
        && \
    python -m ipykernel.kernelspec

# Set up notebook config
COPY jupyter_notebook_config.py /root/.jupyter/

# Jupyter has issues with being run directly: https://github.com/ipython/ipython/issues/7062
COPY run_jupyter.sh /root/

# Expose Ports for Tensorboard (6006) and Ipython (8888)
EXPOSE 6006 8888

WORKDIR "/root"
CMD ["/bin/bash"]