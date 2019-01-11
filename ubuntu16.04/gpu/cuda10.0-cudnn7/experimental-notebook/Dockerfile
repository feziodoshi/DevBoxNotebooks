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

  RUN conda install -c conda-forge ipywidgets \
   && conda install -c conda-forge jupyter_contrib_nbextensions \
   && conda install -c conda-forge jupyter_nbextensions_configurator \
   && jupyter labextension install @jupyter-widgets/jupyterlab-manager

  # RUN git clone https://github.com/lckr/jupyterlab-variableInspector \
  #   && cd jupyterlab-variableInspector && npm install \
  #   && npm run build && jupyter labextension install .

  RUN jupyter labextension install @jupyterlab/plotly-extension \
    && jupyter labextension install @mflevine/jupyterlab_html \
    && jupyter labextension install jupyterlab-drawio \
    && jupyter labextension install jupyterlab-scales \
    && jupyter labextension install jupyterlab_bokeh


# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
