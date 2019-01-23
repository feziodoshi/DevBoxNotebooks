# cocoflow/tensorflow-keras:latest
# Tensorflow + Keras jupyter notebook with gpu suppport (nvidia CUDA cuDNN).

# Build Command:
# > sudo nvidia-docker build -t cocoflow/tensorflow-keras:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/tfminimal-notebook

#FROM grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7
#FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04
#FROM jupyter/scipy-notebook
FROM tensorflow/tensorflow:nightly-gpu-py3-jupyter
#FROM ncvr.io/nvidia/tensorflow:18.12-py3
LABEL maintainer="Colin Conwell <conwell@g.harvard.edu>"

#USER root

USER root

EXPOSE 8888
WORKDIR $HOME/work

# Configure container startup
ENTRYPOINT ["tini", "-g", "--"]
CMD ["start-notebook.sh"]

# Add local files as late as possible to avoid cache busting
COPY start.sh /usr/local/bin/
COPY start-notebook.sh /usr/local/bin/
COPY start-singleuser.sh /usr/local/bin/
COPY jupyter_notebook_config.py /etc/jupyter/
COPY genpass.py .
RUN fix-permissions /etc/jupyter/

WORKDIR $HOME/work

# RUN apt-get update && apt-get install -y --no-install-recommends gnupg2 curl ca-certificates && \
#     curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub | apt-key add - && \
#     echo "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list && \
#     echo "deb https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list && \
#     apt-get purge --autoremove -y curl && \
#     rm -rf /var/lib/apt/lists/*
#
# ENV CUDA_VERSION 10.0.130
#
# ENV CUDA_PKG_VERSION 10-0=$CUDA_VERSION-1
# # For libraries in the cuda-compat-* package: https://docs.nvidia.com/cuda/eula/index.html#attachment-a
# RUN apt-get update && apt-get install -y --no-install-recommends \
#         cuda-cudart-$CUDA_PKG_VERSION \
#         cuda-compat-10-0=410.48-1 && \
#     ln -s cuda-10.0 /usr/local/cuda && \
#     rm -rf /var/lib/apt/lists/*
#
# ENV PATH /usr/local/cuda/bin:${PATH}
#
# # nvidia-container-runtime
# ENV NVIDIA_VISIBLE_DEVICES all
# ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
# ENV NVIDIA_REQUIRE_CUDA "cuda>=10.0 brand=tesla,driver>=384,driver<385"

#RUN pip install tensorflow-gpu keras

WORKDIR /home/$NB_USER/work/
USER $NB_USER
