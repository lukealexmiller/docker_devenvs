FROM tensorflow_src:latest

RUN pip install h5py pillow pyyaml tqdm lxml

RUN mkdir /root/keras && \
	git clone https://github.com/fchollet/keras.git /root/keras && \
	cd /root/keras && \
	python setup.py install

WORKDIR /root

RUN ["/bin/bash"]