# cocoflow/tensorflow-keras:latest
# Tensorflow + Keras jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/tensorflow-keras:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/tfkeras-v1.9-notebook

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-tensorflow-v1.9-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  Update Pip & Conda
# ==============================================================

RUN pip install --upgrade pip \
 && conda update conda

 # ==============================================================
 #  INSTALL KERAS
 # ==============================================================

 RUN pip install --upgrade keras

 # ==============================================================
 #  Upgrade Jupyterlab for Widgets
 # ==============================================================

 RUN pip install jupyterlab==0.32.1 \
   && conda install -c conda-forge ipywidgets \
   && jupyter labextension install @jupyter-widgets/jupyterlab-manager

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
