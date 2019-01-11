# cocoflow/crockpot:ubuntu16.04-cuda9.1-cudnn7
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Instructions:
# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/experimental-notebook

FROM cocoflow/crockpot:latest

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL OPENAI-GYM
# ==============================================================

RUN apt-get -y update && \
  apt-get install -y python-pyglet python3-opengl zlib1g-dev \
  libjpeg-dev patchelf cmake swig libboost-all-dev \
  libsdl2-dev libosmesa6-dev xvfb ffmpeg
RUN pip install --upgrade gym \
  && pip install 'gym[atari]' \
  && pip install 'gym[box2d]'

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================


WORKDIR /home/$NB_USER/work/
USER $NB_USER
