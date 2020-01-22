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

RUN R -e "install.packages('pacman', quiet = TRUE, repos = 'http://cran.us.r-project.org')"

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

RUN pip install jupyterlab-git jupyterlab_latex ipympl

RUN jupyter labextension install jupyterlab-python-file --no-build && \
  jupyter labextension install jupyter-matplotlib --no-build && \
  jupyter labextension install jupyterlab-drawio --no-build && \
  jupyter labextension install jupyterlab_bokeh --no-build && \
  jupyter labextension install @jupyterlab/latex --no-build && \
  jupyter labextension install @jupyterlab/toc --no-build && \
  jupyter labextension install @jupyterlab/git --no-build && \
  jupyter serverextension enable --py jupyterlab_git && \
  jupyter lab build && npm cache clean --force && \
  rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
  rm -rf /home/$NB_USER/.cache/yarn && \
  rm -rf /home/$NB_USER/.node-gyp && \
  fix-permissions $CONDA_DIR && \
  fix-permissions /home/$NB_USER

# RUN git clone https://github.com/ColinConwell/VariableInspector \
#  && cd VariableInspector && npm install \
#  && npm run build && jupyter labextension install .

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
#  Additional Installs
# ==============================================================

RUN conda install -c conda-forge tmux

# ==============================================================
#  Set workdir and user for Jupyter use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
