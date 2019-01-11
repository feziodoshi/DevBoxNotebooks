# cocoflow/fastai:latest
# FastAI jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/fastai:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/fastai-latest-notebook

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda10.0-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL FASTAI
# ==============================================================

RUN pip install pip -U \
  && conda update conda
RUN conda install -c pytorch pytorch-nightly cuda100 \
  && conda install -c fastai torchvision-nightly
RUN conda install -c fastai fastai
#RUN git clone https://github.com/fastai/fastai.git

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
