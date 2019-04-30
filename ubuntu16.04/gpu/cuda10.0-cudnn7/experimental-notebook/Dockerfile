# cocoflow/experimental:latest
# Jupyter notebook with gpu suppport (nvidia CUDA cuDNN), and various frameworks installed.

# Build Command:
# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/experimental-notebook

FROM cocoflow/crockpot:latest

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# # ==============================================================
# #  Install Jupyterlab Extensions
# # ==============================================================
#
# RUN conda install -c conda-forge widgetsnbextension \
#  && conda install -c conda-forge jupyter_contrib_nbextensions \
#  && conda install -c conda-forge jupyter_nbextensions_configurator
#
# RUN jupyter labextension install @jupyterlab/plotly-extension \
#  && jupyter labextension install @mflevine/jupyterlab_html \
#  && jupyter labextension install jupyterlab-drawio \
#  && jupyter labextension install jupyterlab-scales
#  #&& jupyter labextension install dask-labextension \
#  #&& jupyter labextension install jupyterlab_voyager
#
# RUN git clone https://github.com/ColinConwell/jupyterlab-variableInspector \
#  && cd jupyterlab-variableInspector && npm install \
#  && npm run build && jupyter labextension install .
#
# RUN pip install ipympl && jupyter labextension install jupyter-matplotlib
# #
# # ==============================================================
# #  Install R Kernel
# # ==============================================================
#
# RUN apt-get update && \
#   apt-get install -y --no-install-recommends \
#   fonts-dejavu \
#   tzdata \
#   gfortran \
#   gcc && \
#   rm -rf /var/lib/apt/lists/*
#
# # R packages
# RUN conda install --quiet --yes \
#   'r-base' \
#   'r-irkernel' \
#   'r-plyr' \
#   'r-devtools' \
#   'r-tidyverse' \
#   'r-shiny' \
#   'r-rmarkdown' \
#   'r-forecast' \
#   'r-rsqlite' \
#   'r-reshape2' \
#   'r-caret' \
#   'r-rcurl' \
#   'r-crayon' \
#   'r-randomforest' \
#   'r-htmltools' \
#   'r-sparklyr' \
#   'r-htmlwidgets' \
#   'r-hexbin' \
#   'r-ggfortify' \
#   'r-bootstrap' \
#   'r-data.table' \
#   'r-corrplot' \
#   'r-lmerTest' \
#   'r-glmnet' \
#   'r-lattice' \
#   'r-pROC' \
#   'r-psych' \
#   && conda clean -tipsy
#
# # ==============================================================
# #  Install Jupyterlab-Git
# # ==============================================================
#
# RUN apt-get -y update && apt-get install -y openssh-client
#
# RUN jupyter labextension install @jupyterlab/git && pip install jupyterlab-git \
#   && jupyter serverextension enable --py jupyterlab_git
#
# #Installing Git LFS
# RUN apt-get install -y software-properties-common \
#   && add-apt-repository ppa:git-core/ppa \
#   && apt-get update

# RUN build_deps="curl" && apt-get update && \
#       apt-get install -y --no-install-recommends ${build_deps} ca-certificates && \
#       curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
#       apt-get install -y --no-install-recommends git-lfs && \
#       git lfs install && apt-get purge -y --auto-remove ${build_deps} && \
#       rm -r /var/lib/apt/lists/*

# RUN curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash
# RUN apt-get update && apt-get install git-lfs

# RUN git config --global user.name "colinconwell" && \
#   git config --global user.email "colinconwell@gmail.com"
#   git config --global --add url."git@github.com:".insteadOf "https://github.com/"

#RUN apt-get -y update && apt-get install -y openssh-client
#RUN mkdir -p /home/jovyan/.ssh
#RUN ssh-keygen -f /home/jovyan/.ssh/id_rsa -t rsa -N ''
#RUN export GIT_SSH_COMMAND="ssh -i /home/jovyan/.ssh/id_rsa"
#RUN ssh-keyscan github.com >> /home/jovyan/.ssh/known_hosts
#RUN eval `ssh-agent -s` && ssh-add /home/jovyan/.ssh/id_rsa

# ==============================================================
#  INSTALL BLENDER
# ==============================================================

RUN apt-get update && \
	apt-get install -y \
		curl \
		bzip2 \
		libfreetype6 \
		libgl1-mesa-dev \
		libglu1-mesa \
		libxi6 \
		libxrender1 && \
	apt-get -y autoremove && \
	rm -rf /var/lib/apt/lists/*

#ENV BLENDER_MAJOR 2.79
#ENV BLENDER_VERSION 2.79b
#ENV BLENDER_BZ2_URL https://mirror.clarkson.edu/blender/release/Blender$BLENDER_MAJOR/blender-$BLENDER_VERSION-linux-glibc219-x86_64.tar.bz2
ENV BLENDER_BZ2_URL https://mirror.clarkson.edu/blender/release/Blender2.79/blender-2.79b-linux-glibc219-x86_64.tar.bz2

RUN mkdir /usr/local/blender && \
	curl -SL "$BLENDER_BZ2_URL" -o blender.tar.bz2 && \
	tar -jxvf blender.tar.bz2 -C /usr/local/blender --strip-components=1 && \
	rm blender.tar.bz2

ENV PATH /usr/local/blender:$PATH

RUN /usr/local/blender/2.79/python/bin/python3.5m -m ensurepip & \
  /usr/local/blender/2.79/python/bin/python3.5m -m pip install scipy


# ==============================================================
#  INSTALL OPENAI-GYM
# ==============================================================

# RUN apt-get -y update && \
#   apt-get install -y python-pyglet python3-opengl zlib1g-dev \
#   libjpeg-dev patchelf cmake swig libboost-all-dev \
#   libsdl2-dev libosmesa6-dev xvfb ffmpeg
# RUN pip install --upgrade gym \
#   && pip install 'gym[atari]' \
#   && pip install 'gym[box2d]'

# ==============================================================
#  Fixing permissions for R installations
# ==============================================================

#RUN chmod 600 /opt/conda/lib/R
#RUN adduser $NB_USER staff

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
