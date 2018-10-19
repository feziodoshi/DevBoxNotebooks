# cocoflow/fastai-0.7.0:ubuntu16.04-cuda9.1-cudnn7
#
# FastAI 0.7.0 jupyter notebook with gpu suppport (nvidia CUDA cuDNN).
#
# ssh into apophenia (or whatever you machine is named)
# sudo nvidia-docker build -t cocoflow/fastai-0.7.0:ubuntu16.04-cuda9.1-cudnn7 https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda9.1-cudnn7/fastai-v0.7-notebook
#
# test it
# nvidia-docker run -it --rm cocoflow/pytorch:ubuntu16.04-cuda9.1-cudnn7 nvidia-smi

FROM grez72/deep-notebooks:ubuntu16.04-gpu-cuda9.1-cudnn7-minimal-notebook

MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  INSTALL FASTAI
# ==============================================================

LABEL com.nvidia.volumes.needed="nvidia_driver"

RUN echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64 /" > /etc/apt/sources.list.d/nvidia-ml.list

RUN apt-get update && apt-get install -y --no-install-recommends \
         build-essential \
         cmake \
         git \
         curl \
         vim \
         ca-certificates \
         libnccl2=2.2.13-1+cuda9.0 \
         libnccl-dev=2.2.13-1+cuda9.0 \
         python-qt4 \
         libjpeg-dev \
	 zip \
	 unzip \
         libpng-dev &&\
     rm -rf /var/lib/apt/lists/*

RUN conda update conda

WORKDIR /home/jovyan/work

RUN git clone https://github.com/fastai/fastai.git .
RUN ls && /opt/conda/bin/conda env create
RUN /opt/conda/bin/conda clean -ya

ENV PATH /opt/conda/envs/fastai/bin:$PATH
ENV LD_LIBRARY_PATH /usr/local/nvidia/lib:/usr/local/nvidia/lib64
ENV USER fastai

CMD source activate fastai
CMD source ~/.bashrc

WORKDIR /data

RUN curl http://files.fast.ai/data/dogscats.zip --output dogscats.zip
RUN unzip -d . dogscats.zip
RUN rm dogscats.zip

RUN ln -s /data/ /home/jovyan/work/courses/dl1/
RUN ls -la /home/jovyan/work/fastai/courses/dl1/data/

RUN chmod -R a+w /home/jovyan/work

ENV PATH /opt/conda/bin:$PATH
WORKDIR /home/jovyan/work

ENV PATH /opt/conda/envs/fastai/bin:$PATH

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
