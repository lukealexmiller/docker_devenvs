FROM lukemiller/docker:tensorflow

MAINTAINER Luke Miller

# http://ubuntuhandbook.org/index.php/2016/09/install-ffmpeg-3-1-ubuntu-16-04-ppa/
RUN add-apt-repository ppa:jonathonf/ffmpeg-3

# Faster R-CNN has OpenCV dependency
RUN apt-get update && apt-get install -y --no-install-recommends \
        cmake \
        ffmpeg libav-tools x264 x265 \
        git \
        gfortran \
        libatlas-base-dev liblapacke-dev libopenblas-dev \
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

# Clone Faster R-CNN WITH recursion (now using lukealexmiller's fork)
RUN git clone --recursive -b hardneg_mining https://github.com/lukealexmiller/TFFRCNN.git /root/faster_rcnn

# Build Faster R-CNN
RUN cd /root/faster_rcnn/lib && make -j $(($(nproc) + 1))

# Set up notebook config
COPY jupyter_notebook_config.py /root/.jupyter/

# Jupyter has issues with being run directly: https://github.com/ipython/ipython/issues/7062
COPY run_jupyter.sh /root/

# Expose Ports for Tensorboard (6006) and Ipython (8888)
EXPOSE 6006 8888

WORKDIR "/root"
CMD ["/bin/bash"]