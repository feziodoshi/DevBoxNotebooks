# cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7
#
# Pytorch 0.4.0 jupyter notebook with gpu suppport (nvidia CUDA cuDNN).
#
# ssh into apophenia (or whatever you machine is named)
# sudo nvidia-docker build -t cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/pytorch-latest-notebook
#
# test it
# nvidia-docker run -it --rm cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7 nvidia-smi

#FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook
FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-tensorflow-v1.9-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

==============================================================
 INSTALL PYTORCH
==============================================================

RUN pip install --upgrade pip \
  && conda update conda \
  && conda install -y mkl -c intel \
  && conda install pytorch torchvision cuda91 -c pytorch

ENV CUDA_​DEVICE_​ORDER PCI_BUS_ID
ENV CUDA_VISIBLE_DEVICES 0,1

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
