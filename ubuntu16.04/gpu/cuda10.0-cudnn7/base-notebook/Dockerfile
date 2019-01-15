# grez72/base-notebook for deep learning
#
# Runs a fully-loaded jupyterlab notebook server with access to the GPU on
# the host machine, running Ubuntu 16.04, CUDA 10.0, CUDNN7, python3,
# and including most of the packages you might need for deep learning,
# experiments except a deep learning framework! (we build those from this image)
#
# The image begins with nvidia/cuda image, then adds in jupyter/docker-stacks/scipy-notebook
# https://github.com/jupyter/docker-stacks/tree/master/scipy-notebook
#
# The Jupyter server uses a password specified in jupyter_notebook_config.py
# To use tokens instead, comment out the password line of the config file:
# c.NotebookApp.password = 'sha1:b43e2d39955f:b09acc36be8f4d051f6922e10f024db6d7dddd4b'
#
# You can then generate your own password by running the following in python:
# from notebook.auth import passwd
# print(passwd('supersecretpassword'))
#
# Then update the config file:
# c.NotebookApp.password = '...'
#
# uncomment the password line, and rebuild the image (fast because the config
# is copied at the end). You should then be able to login with your password.
#
# build it:
# docker build -t grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7 https://github.com/grez72/notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/base-notebook
#
# test it:
# docker run -it --rm grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7 nvidia-smi
#
# generate a password:
# nvidia-docker run -it --rm grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7 python genpass.py -p 'supersecretpassword'
#
# mount it:
# docker run -it --rm grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7 bash
#
# run it:
# docker run -it --rm -p 8888:8888 grez72/base-notebook:ubuntu16.04-cuda10.0-cudnn7
#
# If you are using jupyterhub, this image can be used with Dockerspawner to
# spawn a single-user notebook.

FROM nvidia/cuda:10.0-cudnn7-devel-ubuntu16.04

LABEL maintainer="Colin Conwell <conwell@g.harvard.edu>"

USER root

#--------------------------------------
#   OS dependencies
#--------------------------------------

ENV DEBIAN_FRONTEND noninteractive
RUN apt-get update && apt-get -yq dist-upgrade \
 && apt-get install -yq --no-install-recommends \
    wget \
    bzip2 \
    ca-certificates \
    sudo \
    locales \
    fonts-liberation \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
    locale-gen

#--------------------------------------
#   Configure environment
#--------------------------------------

ENV CONDA_DIR=/opt/conda \
    SHELL=/bin/bash \
    NB_USER=jovyan \
    NB_UID=1000 \
    NB_GID=100 \
    LC_ALL=en_US.UTF-8 \
    LANG=en_US.UTF-8 \
    LANGUAGE=en_US.UTF-8 \
    JUPYTER_ENABLE_LAB=1
ENV PATH=$CONDA_DIR/bin:$PATH \
    HOME=/home/$NB_USER

ADD fix-permissions /usr/local/bin/fix-permissions
# RUN chmod u+x /usr/local/bin/fix-permissions
# Create jovyan user with UID=1000 and in the 'users' group
# and make sure these dirs are writable by the `users` group.
RUN groupadd wheel -g 11 && \
    echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
    useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
    mkdir -p $CONDA_DIR && \
    chown $NB_USER:$NB_GID $CONDA_DIR && \
    chmod g+w /etc/passwd && \
    fix-permissions $HOME && \
    fix-permissions $CONDA_DIR

# Setup work directory
USER $NB_UID

RUN mkdir /home/$NB_USER/work && \
    fix-permissions /home/$NB_USER

#--------------------------------------
#   Install conda / python
#--------------------------------------

# Install conda as NB_USER and check the md5 sum provided on the download site
ENV MINICONDA_VERSION 4.5.4
RUN cd /tmp && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    echo "a946ea1d0c4a642ddf0c3a26a18bb16d *Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh" | md5sum -c - && \
    /bin/bash Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    $CONDA_DIR/bin/conda config --system --prepend channels conda-forge && \
    $CONDA_DIR/bin/conda config --system --set auto_update_conda false && \
    $CONDA_DIR/bin/conda config --system --set show_channel_urls true && \
    $CONDA_DIR/bin/conda install --quiet --yes conda="${MINICONDA_VERSION%.*}.*" && \
    $CONDA_DIR/bin/conda update --all --quiet --yes && \
    conda clean -tipsy && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Install Tini
RUN conda install --quiet --yes 'tini=0.18.0' && \
    conda list tini | grep tini | tr -s ' ' | cut -d ' ' -f 1,2 >> $CONDA_DIR/conda-meta/pinned && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

#--------------------------------------
#   Install Jupyter Notebook, Lab, and Hub
#--------------------------------------

USER $NB_USER

# Generate a notebook server config
# Cleanup temporary files
# Correct permissions
# Do all this in a single RUN command to avoid duplicating all of the
# files across image layers when the permissions change
RUN conda install --quiet --yes \
    'notebook=5.7.*' \
    'jupyterhub=0.9.*' \
    'jupyterlab=0.35.*' && \
    conda clean -tipsy && \
    jupyter labextension install @jupyterlab/hub-extension@^0.12.0 && \
    npm cache clean --force && \
    jupyter notebook --generate-config && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

#--------------------------------------
# Install all OS dependencies for fully functional notebook server:
#--------------------------------------

USER root

RUN apt-get update && apt-get install -yq --no-install-recommends \
    build-essential \
    emacs \
    git \
    inkscape \
    jed \
    libsm6 \
    libxext-dev \
    libxrender1 \
    lmodern \
    netcat \
    pandoc \
    python-dev \
    texlive-fonts-extra \
    texlive-fonts-recommended \
    texlive-generic-recommended \
    texlive-latex-base \
    texlive-latex-extra \
    texlive-xetex \
    unzip \
    nano \
    && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

#--------------------------------------
# FFMEG with support for: with libx264 (mp4), Nvidia NVENC/NVDEC,
#   and cuda hardware acceleration:
#   https://trac.ffmpeg.org/wiki/HWAccelIntro
#   https://trac.ffmpeg.org/wiki/HWAccelIntro#NVENC
#--------------------------------------

USER root

RUN apt-get update -qq && sudo apt-get -y install \
      autoconf \
      automake \
      build-essential \
      cmake \
      git-core \
      libass-dev \
      libfreetype6-dev \
      libsdl2-dev \
      libtool \
      libva-dev \
      libvdpau-dev \
      libvorbis-dev \
      libxcb1-dev \
      libxcb-shm0-dev \
      libxcb-xfixes0-dev \
      pkg-config \
      texinfo \
      wget \
      zlib1g-dev \
      yasm \
      nasm \
      libx264-dev \
      libx265-dev libnuma-dev \
      libvpx-dev \
      libfdk-aac-dev \
      libmp3lame-dev \
      libopus-dev \
    && rm -rf /var/lib/apt/lists/*

ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=compute,utility,video

# double check to make sure dependencies installed
RUN apt-get -y update && apt-get install -y wget nano git build-essential yasm pkg-config

# install nvidia headers for nvidia codecs
RUN git clone https://github.com/FFmpeg/nv-codec-headers /root/nv-codec-headers && \
  cd /root/nv-codec-headers &&\
  make -j$(nproc) && \
  make install -j$(nproc) && \
  cd /root && rm -rf nv-codec-headers

RUN git clone https://github.com/FFmpeg/FFmpeg /root/ffmpeg \
  && cd /root/ffmpeg \
  && PATH="~/bin:$PATH" PKG_CONFIG_PATH="/root/ffmpeg/lib/pkgconfig" ./configure \
  --prefix="/root/ffmpeg" \
  --pkg-config-flags="--static" \
  --extra-cflags="-I/root/ffmpeg/include" \
  --extra-ldflags="-L/root/ffmpeg/lib" \
  --extra-libs="-lpthread -lm" \
  --bindir="/bin" \
  --disable-shared \
  --enable-avresample \
  --enable-gpl \
  --enable-libass \
  --enable-libfdk-aac \
  --enable-libfreetype \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  --enable-libvpx \
  --enable-libx264 \
  --enable-libx265 \
  --enable-nonfree \
  --enable-cuda \
  --enable-cuvid \
  --enable-nvenc \
  --enable-nonfree \
  --enable-libnpp \
  --extra-cflags=-I/usr/local/cuda/include \
  --extra-cflags=-I/usr/local/include \
  --extra-ldflags=-L/usr/local/cuda/lib64 \
  --extra-libs="-lpthread -lm" \
  && make -j$(nproc) \
  && make install -j$(nproc) \
  && cd /root && rm -rf ffmpeg

#--------------------------------------
# Setup Python 3 Environment
#--------------------------------------

USER $NB_UID
WORKDIR $HOME

# Install Python 3 packages
# Remove pyqt and qt pulled in for matplotlib since we're only ever going to
# use notebook-friendly backends in these images
RUN conda install --quiet --yes \
    'av' \
    'conda-forge::blas=*=openblas' \
    'ipywidgets=7.2*' \
    'pandas=0.23*' \
    'numexpr=2.6*' \
    'matplotlib=2.2*' \
    'scipy=1.1*' \
    'seaborn=0.9*' \
    'scikit-learn=0.19*' \
    'scikit-image=0.14*' \
    'sk-video=1.1*' \
    'sympy=1.1*' \
    'cython=0.28*' \
    'patsy=0.5*' \
    'statsmodels=0.9*' \
    'cloudpickle=0.5*' \
    'dill=0.2*' \
    'numba=0.38*' \
    'bokeh=0.12*' \
    'sqlalchemy=1.2*' \
    'hdf5=1.10*' \
    'h5py=2.7*' \
    'vincent=0.4.*' \
    'beautifulsoup4=4.6.*' \
    'protobuf=3.*' \
    'xlrd'  && \
    conda remove --quiet --yes --force qt pyqt && \
    conda clean -tipsy && \
    jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager@^0.38.1 && \
    jupyter labextension install jupyterlab_bokeh@^0.6.3 && \
    npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Install facets which does not have a pip or conda package at the moment
RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    cd && \
    rm -rf /tmp/facets && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions /home/$NB_USER

#--------------------------------------
#   Updates needed for opencv
#--------------------------------------

RUN apt-get update && apt-get install -y cmake \
  libjpeg8-dev libtiff5-dev libjasper-dev libpng12-dev \
  libgtk2.0-dev \
  libavcodec-dev libavformat-dev libswscale-dev libv4l-dev \
  libatlas-base-dev gfortran \
  libhdf5-serial-dev \
  python3-dev

#--------------------------------------
# Install OpenCV
#--------------------------------------

WORKDIR ~/

# try to force use of our custom installation of ffmpeg
RUN conda uninstall -y ffmpeg

RUN git clone https://github.com/Itseez/opencv.git \
  && cd opencv \
  && git checkout tags/3.4.3 \
  && mkdir build \
  && cd build/ \
  && cmake -D CMAKE_BUILD_TYPE=RELEASE \
    -D PYTHON3_EXECUTABLE:FILEPATH=/opt/conda/bin/python3.6 \
    -D PYTHON3_INCLUDE_DIR:PATH=/opt/conda/include/python3.6m \
    -D PYTHON3_NUMPY_INCLUDE_DIRS:PATH=/opt/conda/lib/python3.6/site-packages/numpy/core/include \
    -D PYTHON3_PACKAGES_PATH:PATH=/opt/conda/lib/python3.6/site-packages \
    -D CMAKE_INSTALL_PREFIX=/usr/local \
    -D BUILD_opencv_dnn=OFF \
    -D BUILD_opencv_dnn_modern=OFF \
    -D BUILD_PROTOBUF=OFF \
    -D WITH_CUDA=OFF \
    -D FORCE_VTK=ON \
    -D WITH_TBB=ON \
    -D WITH_V4L=ON \
    -D WITH_OPENGL=ON \
    -D ENABLE_FAST_MATH=1 \
    -D CUDA_FAST_MATH=1 \
    -D WITH_CUBLAS=1 \
    -D WITH_FFMPEG=1 \
    -D INSTALL_PYTHON_EXAMPLES=ON \
    -D BUILD_EXAMPLES=OFF .. \
  && make -j$(nproc) && sudo make install \
  && ldconfig

#--------------------------------------
#   LIBJPEGTURBO
#--------------------------------------

RUN cd /opt \
    && wget https://downloads.sourceforge.net/libjpeg-turbo/libjpeg-turbo-2.0.0.tar.gz \
    && tar -xzf libjpeg-turbo-2.0.0.tar.gz \
    && rm libjpeg-turbo-2.0.0.tar.gz \
    && cd libjpeg-turbo-2.0.0 \
    && mkdir build \
    && cd build \
    && cmake -DCMAKE_INSTALL_PREFIX=/usr/libjpeg-turbo \
    -DCMAKE_BUILD_TYPE=RELEASE  \
    -DENABLE_STATIC=FALSE       \
    -DCMAKE_INSTALL_DOCDIR=/usr/share/doc/libjpeg-turbo-2.0.0 \
    -DCMAKE_INSTALL_DEFAULT_LIBDIR=lib  \
    .. \
    && make -j"$(nproc)" \
    && make test \
    && make install \
    && mkdir -p /opt/libjpeg-turbo/lib64 \
    && ln -s /usr/libjpeg-turbo/lib/libturbojpeg.so /opt/libjpeg-turbo/lib64/libturbojpeg.so

#--------------------------------------
# PyTurboJPEG
#--------------------------------------

# http://blog.loopbio.com/video-io-2-jpeg-decoding.html
# using loopbio's PyTurbojpeg fork! https://github.com/loopbio/PyTurbojpeg
RUN pip install -U git+git://github.com/loopbio/PyTurboJPEG.git

#--------------------------------------
# Utilities for optimizing python code:
#--------------------------------------

# Cupy, PyCuda, Numba
RUN conda install -y -c lukepfister pycuda \
  && pip install cupy \
  && conda install -y numba

#--------------------------------------
# Install Video and Image Utilities
#--------------------------------------

# Installing all of these dependencies together in one RUN command
# to avoid limit on number of layers in docker image
RUN conda install -y -c conda-forge av trackpy ipywidgets \
  && conda install -y filterpy \
  && jupyter nbextension enable --py --sys-prefix widgetsnbextension \
  && pip install --upgrade pip \
  && pip install \
    ffmpeg-python \
    image-attendant \
    imutils \
    imageio \
    https://github.com/soft-matter/pims/archive/master.zip \
    pynvvl-cuda92 \
    sk-video \
    tqdm \
  && cd $HOME \
  && git clone https://github.com/DheerendraRathor/ffprobe3 \
  && cd $HOME/ffprobe3 \
  && python setup.py install \
  && cd $HOME \
  && git clone https://github.com/jfrelinger/cython-munkres-wrapper \
  && cd $HOME/cython-munkres-wrapper \
  && python setup.py install

#--------------------------------------
# Parallel HDF5
#--------------------------------------

# https://drtiresome.com/2016/08/23/build-and-install-mpi-parallel-hdf5-and-h5py-from-source-on-linux/

WORKDIR /tmp

# install MPICH, so we can pip install mpi4py
RUN wget http://www.mpich.org/static/downloads/3.2.1/mpich-3.2.1.tar.gz \
  && tar -xzf mpich-3.2.1.tar.gz \
  && cd mpich-3.2.1 \
  && ./configure --enable-romio --enable-shared --with-device=ch3:sock --disable-fortran --prefix=/usr/local \
  && make -j$(nproc) \
  && make install \
  && make clean

# install mpi4py
RUN pip install mpi4py

# https://github.com/HDFGroup/
# h5cc -showconfig
RUN wget https://support.hdfgroup.org/ftp/HDF5/releases/hdf5-1.10/hdf5-1.10.3/src/hdf5-1.10.3.tar.gz \
  && tar -xzf hdf5-1.10.3.tar.gz \
  && cd hdf5-1.10.3 \
  && ./configure --prefix=/usr/local --enable-parallel --enable-shared --enable-build-mode=production \
  && make -j$(nproc) \
  && make install \
  && make clean

#--------------------------------------
# Install Additional Packages
#--------------------------------------

# plotly + dependencies
RUN apt-get update && apt-get install -y build-essential libcap-dev
RUN easy_install python-prctl
RUN pip install plotly msgpack msgpack-numpy lmdb \
  && pip install --upgrade git+https://github.com/tensorpack/tensorpack.git \
  && conda install -c bioconda cufflinks

RUN pip install wget pathlib \
  && conda install -y cookiecutter \
  && conda install -c menpo pathlib \
  && conda clean -tipsy

#--------------------------------------
# Install Optimized Fast PIL
#--------------------------------------

RUN conda uninstall --force jpeg libtiff -y \
  && pip uninstall -y pillow \
  && conda install -y -c conda-forge libjpeg-turbo \
  && CC="cc -mavx2" pip install --no-cache-dir -U --force-reinstall pillow-simd

#--------------------------------------
# Notebook Setup
#--------------------------------------

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

# Switch back to jovyan to avoid accidental container runs as root
