FROM lukemiller/docker:tensorflow_frcnn

MAINTAINER Luke Miller

# Expose Ports for Tensorboard (6006) and Ipython (8888)
EXPOSE 6006

COPY face_blur.sh /root/

WORKDIR "/root"

ENTRYPOINT ["/bin/bash"]

CMD ["/root/face_blur.sh"]