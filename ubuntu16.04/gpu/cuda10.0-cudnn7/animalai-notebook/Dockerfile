# cocoflow/animalai:latest
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Command:
# > sudo nvidia-docker build -t cocoflow/animalai:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/animalai-notebook

FROM cocoflow/base-notebook:ubuntu16.04-cuda10.0-cudnn7

# MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# # ==============================================================
# #  Updates & Upgrades
# # ==============================================================

# RUN pip install --upgrade pip \
#   && conda update conda

# # ==============================================================
# #  Install Tensorflow & Keras
# # ==============================================================

# RUN pip install tensorflow-gpu keras

# # ==============================================================
# #  Install PyTorch & Fastai
# # ==============================================================

# RUN pip install --upgrade numpy

# RUN conda install -c conda-forge mock && conda install -y mkl -c intel \
#   && conda install pytorch torchvision cuda100 -c pytorch \
#   && conda clean -tipsy

# RUN pip install fastai fastprogress

# ==============================================================
#  Install Animalai Framework
# ==============================================================

RUN apt-get update && apt-get install -y curl bzip2 xvfb ffmpeg git libxrender1

RUN pip install animalai
RUN pip install animalai-train
RUN conda install --quiet --yes bokeh h5py hdf5 ipywidgets matplotlib numba pandas scikit-image scikit-learn  scipy seaborn 
RUN conda install pytorch torchvision -c pytorch
RUN conda clean --all -f -y

# ==============================================================
#  Set workdir and user for jupyter notebook use.
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
