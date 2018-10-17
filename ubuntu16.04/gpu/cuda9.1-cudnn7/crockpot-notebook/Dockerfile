# cocoflow/crockpot:ubuntu16.04-cuda9.1-cudnn7
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.
# Build Instructions:
# > ssh username@apophenia.wjh.harvard.edud
# > sudo nvidia-docker build -t cocoflow/crockpot:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/crockpot-notebook
# > nvidia-docker run -it --rm cocoflow/crockpot:ubuntu16.04-cuda9.1-cudnn7 nvidia-smi

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL & UPGRADE PIP
# ==============================================================

RUN pip install --upgrade pip

# ==============================================================
#  INSTALL TENSORFLOW
# ==============================================================

RUN pip install --upgrade tensorflow-gpu

# ==============================================================
#  INSTALL KERAS
# ==============================================================

RUN pip install --upgrade keras

# ==============================================================
#  INSTALL PYTORCH
# ==============================================================

RUN conda install pytorch torchvision cuda90 -c pytorch

# ==============================================================
#  INSTALL PYRO
# ==============================================================

RUN pip install --upgrade pyro-ppl

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
