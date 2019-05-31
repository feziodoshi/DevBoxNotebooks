# cocoflow/pytorch:latest
# Pytorch jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/pytorch:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/scratchpad-notebook

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
#  Install Everything
# ==============================================================

RUN git clone --branch tutorial https://github.com/CSAILVision/gandissect.git

#RUN cd gandissect && script/setup_env.sh && script/make_dirs.sh
#RUN source activate netd && pip install -v -e .
RUN cd gandissect && pip install -v -e .

RUN wget https://github.com/ninja-build/ninja/releases/download/v1.8.2/ninja-linux.zip \
  && unzip ninja-linux.zip -d /usr/local/bin/ \
  && update-alternatives --install /usr/bin/ninja ninja /usr/local/bin/ninja 1 --force 

RUN conda install h5py pandas
RUN pip install tqdm moviepy


# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
