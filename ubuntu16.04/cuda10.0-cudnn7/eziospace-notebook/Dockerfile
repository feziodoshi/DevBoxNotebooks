# cocoflow/experimental:latest

# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/cuda10.0-cudnn7/experimental

FROM cocoflow/labspace-stable:latest

USER root
WORKDIR $HOME

#  Install Tensorflow 1.1x
RUN pip install tensorflow-gpu==1.14 tensorboard visdom

WORKDIR /home/$NB_USER/work/
USER $NB_USER
