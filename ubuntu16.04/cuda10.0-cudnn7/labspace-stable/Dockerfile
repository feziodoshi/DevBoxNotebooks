# cocoflow/labspace-stable:latest

# > sudo nvidia-docker build -t cocoflow/labspace:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/cuda10.0-cudnn7/labspace-stable

FROM cocoflow/crockpot:latest

USER root
WORKDIR $HOME

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

RUN pip install jupyterlab-git jupyterlab_latex ipympl && \
  conda install -c conda-forge jupyterlab jupyter_conda

RUN jupyter labextension install jupyterlab-python-file --no-build && \
  jupyter labextension install jupyterlab_toastify --no-build && \
  jupyter labextension install jupyter-matplotlib --no-build && \
  jupyter labextension install jupyterlab-drawio --no-build && \
  jupyter labextension install jupyterlab_bokeh --no-build && \
  jupyter labextension install jupyterlab_conda --no-build && \
  jupyter labextension install @jupyterlab/toc --no-build && \
  jupyter labextension install @jupyterlab/git --no-build && \
  jupyter serverextension enable --py jupyterlab_git && \
  jupyter lab build --dev-build=False --minimize=False && \
  npm cache clean --force && \
  rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
  rm -rf /home/$NB_USER/.cache/yarn && \
  rm -rf /home/$NB_USER/.node-gyp && \
  fix-permissions $CONDA_DIR && \
  fix-permissions /home/$NB_USER

RUN pip install jupyterlab-zip

# RUN git clone https://github.com/ColinConwell/VariableInspector \
#  && cd VariableInspector && npm install \
#  && npm run build && jupyter labextension install .

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
#  Fixing permissions for R installations
# ==============================================================

#RUN chmod 600 /opt/conda/lib/R
#RUN adduser $NB_USER staff

# ==============================================================
#  set workdir and user for jupyter notebook use
# ==============================================================

WORKDIR /home/$NB_USER/work/
USER $NB_USER
