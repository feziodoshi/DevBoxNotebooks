# cocoflow/crockpot:latest
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Command:
# > sudo nvidia-docker build -t cocoflow/crockpot:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/crockpot-notebook

FROM cocoflow/base-notebook:ubuntu16.04-cuda10.0-cudnn7

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  Updates & Upgrades
# ==============================================================

RUN pip install --upgrade pip \
  && conda update conda

RUN pip install --upgrade numpy

# ==============================================================
#  Install Machine Learning Packages
# ==============================================================

RUN apt-get update && \
    apt-get install -y --no-install-recommends ffmpeg && \
    rm -rf /var/lib/apt/lists/*

RUN conda install -c r r-essentials

RUN conda install --quiet --yes \
    'beautifulsoup4' \
    'conda-forge::blas=*=openblas' \
    'bokeh' \
    'cloudpickle' \
    'cython' \
    'dask' \
    'dill' \
    'h5py' \
    'hdf5' \
    'ipywidgets' \
    'matplotlib-base' \
    'numba' \
    'numexpr' \
    'pandas' \
    'patsy' \
    'protobuf' \
    'scikit-image' \
    'scikit-learn' \
    'scipy' \
    'seaborn' \
    'sqlalchemy' \
    'statsmodels' \
    'sympy' \
    'vincent' \
    'xlrd' \
    && conda clean --all -f -y

# ==============================================================
#  Install Tensorflow & Keras
# ==============================================================

RUN pip install tensorflow-gpu keras

# ==============================================================
#  Install PyTorch
# ==============================================================

RUN pip install torch torchvision

# ==============================================================
#  Install FastAI
# ==============================================================

RUN pip install fastai fastprogress

# ==============================================================
#  Install Pyro
# ==============================================================

RUN pip install --upgrade pyro-ppl

# ==============================================================
#  Install R Kernel
# ==============================================================

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    unixodbc \
    unixodbc-dev \
    r-cran-rodbc \
    gfortran \
    gcc && \
    rm -rf /var/lib/apt/lists/*

# R packages
RUN conda install --quiet --yes \
  'r-base' \
  'r-irkernel' \
  'r-plyr' \
  'r-devtools' \
  'r-tidyverse' \
  'r-shiny' \
  'r-rmarkdown' \
  'r-forecast' \
  'r-rsqlite' \
  'r-reshape2' \
  'r-caret' \
  'r-rcurl' \
  'r-crayon' \
  'r-randomforest' \
  'r-htmltools' \
  'r-sparklyr' \
  'r-htmlwidgets' \
  'r-hexbin' \
  'r-ggfortify' \
  'r-bootstrap' \
  'r-data.table' \
  'r-corrplot' \
  'r-lme4' \
  'r-glmnet' \
  'r-lattice' \
  'r-pROC' \
  'r-psych' \
  && conda clean -tipsy

# ==============================================================
#  Set workdir and user for jupyter notebook use.
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
