# cocoflow/experimental:latest
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Command:
# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/experimental-notebook

FROM cocoflow/crockpot:latest

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL OPENAI-GYM
# ==============================================================

RUN apt-get -y update && \
  apt-get install -y python-pyglet python3-opengl zlib1g-dev \
  libjpeg-dev patchelf cmake swig libboost-all-dev \
  libsdl2-dev libosmesa6-dev xvfb ffmpeg
RUN pip install --upgrade gym \
  && pip install 'gym[atari]' \
  && pip install 'gym[box2d]'

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

RUN conda install -c conda-forge widgetsnbextension \
 && conda install -c conda-forge jupyter_contrib_nbextensions \
 && conda install -c conda-forge jupyter_nbextensions_configurator

RUN jupyter labextension install @jupyterlab/plotly-extension \
 && jupyter labextension install @mflevine/jupyterlab_html \
 && jupyter labextension install jupyterlab-drawio \
 && jupyter labextension install jupyterlab-scales \
 && jupyter labextension install @jupyterlab/celltags
 #&& jupyter labextension install jupyterlab_voyager

RUN git clone https://github.com/ColinConwell/jupyterlab-variableInspector \
 && cd jupyterlab-variableInspector && npm install \
 && npm run build && jupyter labextension install .

RUN pip install ipympl && jupyter labextension install jupyter-matplotlib

# ==============================================================
#  Install R Kernel
# ==============================================================

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
  fonts-dejavu \
  tzdata \
  gfortran \
  gcc && \
  rm -rf /var/lib/apt/lists/*

# R packages
RUN conda install --quiet --yes \
  'r-base=3.5.1' \
  'r-irkernel=0.8*' \
  'r-plyr=1.8*' \
  'r-devtools=1.13*' \
  'r-tidyverse=1.2*' \
  'r-shiny=1.2*' \
  'r-rmarkdown=1.11*' \
  'r-forecast=8.2*' \
  'r-rsqlite=2.1*' \
  'r-reshape2=1.4*' \
  'r-nycflights13=1.0*' \
  'r-caret=6.0*' \
  'r-rcurl=1.95*' \
  'r-crayon=1.3*' \
  'r-randomforest=4.6*' \
  'r-htmltools=0.3*' \
  'r-sparklyr=0.9*' \
  'r-htmlwidgets=1.2*' \
  'r-hexbin=1.27*' \
  && conda clean -tipsy

# ==============================================================
#  Install Jupyterlab-Git
# ==============================================================

RUN jupyter labextension install @jupyterlab/git && \
  pip install jupyterlab-git && \
  jupyter serverextension enable --py jupyterlab_git

RUN git config --global user.name "colinconwell" && \
  git config --global --add url."git@github.com:".insteadOf "https://github.com/"

RUN apt-get -y update && apt-get install -y openssh-client
RUN mkdir -p /home/jovyan/.ssh
RUN ssh-keygen -f /home/jovyan/.ssh/id_rsa -t rsa -N ''
RUN export GIT_SSH_COMMAND="ssh -i /home/jovyan/.ssh/id_rsa"

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
