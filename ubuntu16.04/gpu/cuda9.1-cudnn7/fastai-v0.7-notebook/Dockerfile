# cocoflow/fastai-0.7.0:ubuntu16.04-cuda9.1-cudnn7
#
# FastAI 0.7.0 jupyter notebook with gpu suppport (nvidia CUDA cuDNN).
#
# ssh into apophenia (or whatever you machine is named)
# sudo nvidia-docker build -t cocoflow/fastai-0.7.0:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/fastai-v0.7-notebook
#
# test it
# nvidia-docker run -it --rm cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7 nvidia-smi

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL FASTAI
# ==============================================================

WORKDIR /home/$NB_USER/work/

RUN git clone https://github.com/fastai/fastai

WORKDIR /home/$NB_USER/work/fastai

RUN pip install -e .


# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
