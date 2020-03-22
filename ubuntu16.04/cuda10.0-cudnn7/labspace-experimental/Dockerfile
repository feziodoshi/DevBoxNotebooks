# cocoflow/experimental:latest

# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/cuda10.0-cudnn7/labspace-experimental

FROM cocoflow/base-notebook:ubuntu16.04-cuda10.0-cudnn7

USER root
WORKDIR $HOME

# ==============================================================
#  Updates & Upgrades
# ==============================================================

RUN pip install --upgrade pip \
  && conda update -n base conda

#Rollback to a long-term contigent JupyterLab
#RUN conda install --quiet --yes 'jupyterlab==1.0.0'

# ==============================================================
#  Install Machine Learning Packages
# ==============================================================

RUN apt-get update && \
    apt-get install -y --no-install-recommends ffmpeg && \
    rm -rf /var/lib/apt/lists/*

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
    'pymc3' \
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

RUN pip install opencv-python

# ==============================================================
#  Install Deep Learning Packages
# ==============================================================

RUN pip install tensorflow-gpu keras torch torchvision \
  fastai fastprogress pyro-ppl edward

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

RUN conda install -c r r-essentials

# R packages
RUN conda install --quiet --yes \
  'r-tidyverse' \
  && conda clean -tipsy

RUN R -e "install.packages('pacman', quiet = TRUE, repos = 'http://cran.us.r-project.org')"

# ==============================================================
#  Install Blender
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

RUN cp /etc/ssl/certs/ca-certificates.crt /opt/conda/ssl/cacert.pem

ENV BLENDER_BZ2_URL https://mirror.clarkson.edu/blender/release/Blender2.79/blender-2.79b-linux-glibc219-x86_64.tar.bz2

RUN mkdir /usr/local/blender && \
	curl -SL "$BLENDER_BZ2_URL" -o blender.tar.bz2 && \
	tar -jxvf blender.tar.bz2 -C /usr/local/blender --strip-components=1 && \
	rm blender.tar.bz2

ENV PATH /usr/local/blender:$PATH
ENV BLENDER_MODULES /usr/local/blender/2.79/python/lib/python3.5/site-packages/

RUN /usr/local/blender/2.79/python/bin/python3.5m -m ensurepip
RUN /usr/local/blender/2.79/python/bin/python3.5m -m pip install --target='/usr/local/blender/2.79/python/lib/python3.5/site-packages/' \
  --upgrade pip numpy scipy pandas

# ==============================================================
#  Additional Conda / Conda-Forge Installs
# ==============================================================

RUN conda install nb_conda nb_conda_kernels && \
  conda install -c conda-forge tmux

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

RUN conda install -c conda-forge jupyter_conda && \
  pip install jupyterlab-git jupyterlab_latex ipympl

RUN jupyter labextension install --no-build \
  @jupyter-widgets/jupyterlab-manager \
  jupyterlab-python-file \
  jupyterlab_toastify \
  #jupyterlab_voyager \
  jupyter-matplotlib \
  jupyterlab-drawio \
  jupyterlab_bokeh \
  jupyterlab_conda \
  @jupyterlab/latex \
  @jupyterlab/toc \
  @jupyterlab/git && \
  jupyter lab build \
  jupyter lab clean \
  npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
#     jupyter labextension install @jupyter-widgets/jupyterlab-manager --no-build && \
#     jupyter labextension install jupyterlab-python-file --no-build && \
#     jupyter labextension install jupyterlab_toastify --no-build && \
#     #jupyter labextension install jupyterlab_voyager --no-build && \
#     jupyter labextension install jupyter-matplotlib --no-build && \
#     jupyter labextension install jupyterlab-drawio --no-build && \
#     #jupyter labextension install jupyterlab_bokeh --no-build && \
#     jupyter labextension install jupyterlab_conda --no-build && \
#     jupyter labextension install @jupyterlab/latex --no-build && \
#     jupyter labextension install @jupyterlab/toc --no-build && \
#     jupyter labextension install @jupyterlab/git --no-build && \
#     jupyter lab build && \
#     jupyter lab clean && \
#     npm cache clean --force && \
#     rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
#     rm -rf /home/$NB_USER/.cache/yarn && \
#     rm -rf /home/$NB_USER/.node-gyp && \
#     fix-permissions $CONDA_DIR && \
#     fix-permissions /home/$NB_USER

WORKDIR /home/$NB_USER/work/
USER $NB_USER
