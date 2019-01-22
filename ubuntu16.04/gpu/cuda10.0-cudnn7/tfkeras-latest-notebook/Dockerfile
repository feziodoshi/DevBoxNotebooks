# cocoflow/tensorflow-keras:latest
# Tensorflow + Keras jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/tensorflow-keras:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/tfkeras-latest-notebook

FROM nvcr.io/nvidia/tensorflow:18.12-py3

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  Install CONDA
# ==============================================================

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda2-5.3.0-Linux-x86_64.sh -O ~/anaconda.sh \
  && /bin/bash ~/anaconda.sh -b -p /opt/conda && rm ~/anaconda.sh \
  && ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh \
  && echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc \
  && echo "conda activate base" >> ~/.bashrc

 # ==============================================================
 #  INSTALL KERAS
 # ==============================================================

 RUN pip install --upgrade keras

# ==============================================================
#  Set Cuda Variables
# ==============================================================

#ENV CUDA_​DEVICE_​ORDER PCI_BUS_ID
#ENV CUDA_VISIBLE_DEVICES 2,3

#--------------------------------------
#   Jupyterhub/Jupyterlab
#--------------------------------------

# USER root
# RUN pip install jupyter -U && pip install jupyterlab
# COPY jupyter_notebook_config.py /etc/jupyter/
# #COPY genpass.py .
# RUN fix-permissions /etc/jupyter/
#
# WORKDIR $HOME/work
#
# EXPOSE 8888
#
# ENTRYPOINT ["tini", "-g", "--"]
# CMD ["jupyter", "lab","--ip=0.0.0.0","--allow-root"]
#
# # ==============================================================
# #  set workdir and user for jupyter notebook use
# # ==============================================================
#
# WORKDIR /home/$NB_USER/work/
# USER $NB_USER
