# cocoflow/fastai:ubuntu16.04-cuda9.1-cudnn7
# FastAI jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# sudo nvidia-docker build -t cocoflow/fastai:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/fastai-latest-notebook

# nvidia-docker run -it --rm cocoflow/fastai:ubuntu16.04-cuda9.1-cudnn7 nvidia-smi

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL FASTAI
# ==============================================================

RUN pip install pip -U \
  && conda update conda
RUN conda install -c pytorch pytorch-nightly cuda90 \
  && conda install -c fastai torchvision-nightly
RUN conda install -c fastai fastai
#RUN git clone https://github.com/fastai/fastai.git

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
