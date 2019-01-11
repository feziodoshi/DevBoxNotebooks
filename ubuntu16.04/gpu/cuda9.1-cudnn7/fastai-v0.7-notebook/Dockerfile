# cocoflow/fastai-0.7.0:ubuntu16.04-cuda9.1-cudnn7
# FastAI 0.7.0 jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# sudo nvidia-docker build -t cocoflow/fastai-0.7.0:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/fastai-v0.7-notebook

# nvidia-docker run -it --rm cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7 nvidia-smi

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL FASTAI
# ==============================================================

RUN pip install pip -U \
  && conda update conda
RUN pip install fastai==0.7.0
RUN pip install torchtext==0.2.3
#RUN git clone https://github.com/fastai/fastai.git

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
