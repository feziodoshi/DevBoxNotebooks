# cocoflow/experimental:latest

# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/cuda10.0-cudnn7/labspace-experimental

FROM cocoflow/labspace:ubuntu16.04-cuda10.0-cudnn7

USER root
WORKDIR $HOME

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

#Rollback to a long-term contigent JupyterLab
RUN conda install --quiet --yes 'jupyterlab=1.0.0'

RUN pip install jupyterlab-git jupyterlab_latex ipympl

RUN jupyter labextension install @jupyterlab/latex --no-build && \
  jupyter labextension install jupyterlab_voyager --no-build && \
  jupyter labextension install jupyter-matplotlib --no-build && \
  jupyter labextension install jupyterlab-drawio --no-build && \
  jupyter labextension install jupyterlab_bokeh --no-build && \
  jupyter labextension install @jupyterlab/toc --no-build && \
  jupyter labextension install @jupyterlab/git --no-build && \
  jupyter serverextension enable --py jupyterlab_git && \
  jupyter lab build --dev-build=False && \
  npm cache clean --force && \
  rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
  rm -rf /home/$NB_USER/.cache/yarn && \
  rm -rf /home/$NB_USER/.node-gyp && \
  fix-permissions $CONDA_DIR && \
  fix-permissions /home/$NB_USER

WORKDIR /home/$NB_USER/work/
USER $NB_USER
