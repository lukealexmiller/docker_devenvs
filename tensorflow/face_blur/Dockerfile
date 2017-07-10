FROM lukemiller/docker:tensorflow

MAINTAINER Luke Miller

# Faster R-CNN has OpenCV dependency
RUN apt-get update && apt-get install -y --no-install-recommends \
        cmake \
        ffmpeg \
        gfortran \
        libatlas-base-dev \
        libavcodec-dev \
        libavformat-dev \
        libjpeg8-dev \
        libtiff5-dev \
        libjasper-dev \
        libv4l-dev \
        libxvidcore-dev \
        libx264-dev \
        libgtk-3-dev \
        libswscale-dev \
        python-tk \
        qtbase5-dev \
        && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get update && apt-get install -y --no-install-recommends \
        libopencv-dev \
        libdc1394-22 libdc1394-22-dev libjpeg-dev \
        libxine2-dev libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev \
        libtbb-dev libfaac-dev libmp3lame-dev libopencore-amrnb-dev libopencore-amrwb-dev libtheora-dev \
        libvorbis-dev libxvidcore-dev v4l-utils python-vtk \
        liblapacke-dev libopenblas-dev \
        libgdal-dev \
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

RUN wget -O opencv.zip https://github.com/opencv/opencv/archive/3.2.0.zip && unzip opencv.zip

RUN cd /root/opencv-3.2.0/build/ && cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local  \
    -DFORCE_VTK=ON -D WITH_TBB=ON -D WITH_V4L=ON -D WITH_QT=ON -D WITH_OPENGL=ON -D WITH_CUBLAS=ON \
    -D ENABLE_FAST_MATH=1 -D CUDA_FAST_MATH=1 -D CUDA_NVCC_FLAGS="-D_FORCE_INLINES" \
    -D WITH_GDAL=ON -D WITH_XINE=ON -D CUDA_GENERATION=Pascal ..

RUN cd /root/opencv-3.2.0/build/ && make -j $(($(nproc) + 1))

# Set up notebook config
COPY jupyter_notebook_config.py /root/.jupyter/

# Jupyter has issues with being run directly: https://github.com/ipython/ipython/issues/7062
COPY run_jupyter.sh /root/

# Expose Ports for Tensorboard (6006) and Ipython (8888)
EXPOSE 6006 8888

WORKDIR "/root"
CMD ["/bin/bash"]