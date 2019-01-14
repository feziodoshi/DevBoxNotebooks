# cocoflow/pytorch:latest
# Pytorch jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/pytorch-latest-notebook

#FROM grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7
FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  Update Pip & Conda
# ==============================================================

RUN pip install --upgrade pip \
 && conda update conda

# ==============================================================
#  Install PyTorch
# ==============================================================

RUN conda install pytorch=0.4.1 torchvision=0.2.1 cuda90 -c pytorch

# ==============================================================
#  Set Cuda Variables
# ==============================================================

ENV CUDA_​DEVICE_​ORDER PCI_BUS_ID
ENV CUDA_VISIBLE_DEVICES 2,3

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
