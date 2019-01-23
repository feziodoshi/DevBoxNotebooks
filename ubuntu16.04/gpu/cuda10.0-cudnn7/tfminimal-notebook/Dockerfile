# cocoflow/tensorflow-keras:latest
# Tensorflow + Keras jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/tensorflow-keras:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/tfminimal-notebook

FROM grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7
#FROM jupyter/base-notebook
LABEL maintainer="Colin Conwell <conwell@g.harvard.edu>"

WORKDIR /home/$NB_USER/work/
USER $NB_USER
