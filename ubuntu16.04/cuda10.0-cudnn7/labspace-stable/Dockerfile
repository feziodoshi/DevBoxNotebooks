# cocoflow/labspace-stable:latest

# > sudo nvidia-docker build -t cocoflow/labspace:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/cuda10.0-cudnn7/labspace-stable

FROM cocoflow/base-notebook:ubuntu16.04-cuda10.0-cudnn7

USER root
WORKDIR $HOME

# ==============================================================
#  Updates & Upgrades
# ==============================================================

RUN pip install --upgrade pip \
  && conda update conda

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
    'fire' \
    'ipywidgets' \
    'matplotlib-base' \
    'numba' \
    'numexpr' \
    'pandas' \
    'patsy' \
    'pingouin' \
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
#  Install Deep Learning Frameworks
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

RUN R -e "install.packages('pacman', quiet = TRUE, repos = 'http://cran.us.r-project.org')"

# ==============================================================
#  Set workdir and user for Jupyter use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
