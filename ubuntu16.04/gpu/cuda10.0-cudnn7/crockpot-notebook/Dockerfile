# cocoflow/crockpot:latest
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Command:
# > sudo nvidia-docker build -t cocoflow/crockpot:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/crockpot-notebook

FROM grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  Updates & Upgrades
# ==============================================================

RUN pip install --upgrade pip \
  && conda update conda

# ==============================================================
#  Install Tensorflow & Keras
# ==============================================================

RUN pip install tensorflow-gpu keras

# ==============================================================
#  Install PyTorch
# ==============================================================

RUN pip install --upgrade numpy

RUN conda install -c conda-forge mock && conda install -y mkl -c intel \
  && conda install pytorch torchvision cuda100 -c pytorch \
  && conda clean -tipsy

# ==============================================================
#  Install Pyro
# ==============================================================

RUN pip install --upgrade pyro-ppl

# ==============================================================
#  Install FastAI
# ==============================================================

RUN pip install fastai fastprogress

# ==============================================================
#  Set workdir and user for jupyter notebook use.
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
