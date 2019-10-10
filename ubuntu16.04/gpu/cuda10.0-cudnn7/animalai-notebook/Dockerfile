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

# ==============================================================
#  Install Animalai Framework
# ==============================================================

RUN apt-get update && apt-get install -y curl bzip2 xvfb ffmpeg git libxrender1

RUN pip install animalai
RUN pip install animalai-train
RUN pip install torch torchvision
RUN pip install pandas seaborn scikit-learn scikit-image
RUN pip install ipywidgets && jupyter nbextension enable --py widgetsnbextension
RUN jupyter labextension install @jupyter-widgets/jupyterlab-manager


# ==============================================================
#  Set workdir and user for jupyter notebook use.
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
