# cocoflow/crockpot:latest
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Command:
# > sudo nvidia-docker build -t cocoflow/crockpot:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/crockpot-notebook

FROM grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7

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

RUN conda install pytorch torchvision cuda100 -c pytorch

# ==============================================================
#  Install Tensorflow
# ==============================================================

RUN pip install --upgrade tf-nightly-gpu

# ==============================================================
#  Install Keras
# ==============================================================

RUN pip install --upgrade keras

# ==============================================================
#  Install Pyro
# ==============================================================

RUN pip install --upgrade pyro-ppl

# ==============================================================
#  Install FastAI
# ==============================================================

RUN pip install --upgrade fastai

# ==============================================================
#  Set workdir and user for jupyter notebook use.
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
