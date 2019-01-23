# cocoflow/tensorflow-keras:latest
# Tensorflow + Keras jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/tensorflow-keras:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/tfminimal-notebook


FROM python:3.6
RUN pip3 install \
    jupyterhub==0.7.2 \
    'notebook>=5.0,<=6.0'
RUN chmod +x start-singleuser.sh

# create a user, since we don't want to run as root
RUN useradd -m jovyan
ENV HOME=/home/jovyan
WORKDIR $HOME
USER jovyan

CMD ["jupyterhub-singleuser"]



# FROM nvcr.io/nvidia/tensorflow:18.12-py3
# #
# MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>
# #
# ARG JUPYTERHUB_VERSION=0.8.0
# RUN pip3 install --no-cache \
#     jupyterhub==$JUPYTERHUB_VERSION
#
# RUN pip3 install \
#     jupyterhub==0.7.2 \
#     'notebook>=5.0,<=6.0'
#
# # create a user, since we don't want to run as root
# RUN useradd -m jovyan
# ENV HOME=/home/jovyan
# WORKDIR $HOME
#
# COPY notebook_config.py /etc/jupyter/
# COPY genpass.py .
# RUN fix-permissions /etc/jupyter/
#
# USER jovyan
#
# CMD ["jupyterhub-singleuser"]

# FROM python:3.6
#
# RUN pip3 install \
#     jupyterhub==0.7.2 \
#     'notebook>=5.0,<=6.0'
#
# # create a user, since we don't want to run as root
# RUN useradd -m jovyan
# ENV HOME=/home/jovyan
# WORKDIR $HOME
# USER jovyan

# USER jovyan
#
# CMD ["jupyterhub-singleuser"]

# EXPOSE 8888
#
# ENTRYPOINT ["tini", "-g", "--"]
# CMD ["jupyter", "lab","--ip=0.0.0.0","--allow-root"]

#!pip install opencv-python
#!apt update && apt install -y libsm6 libxext6 libxrender-dev
