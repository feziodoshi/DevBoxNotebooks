# cocoflow/experimental:latest

# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/cuda10.0-cudnn7/labspace-experimental

FROM cocoflow/base-notebook:ubuntu16.04-cuda10.0-cudnn7

USER root
WORKDIR $HOME

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

#Rollback to a long-term contigent JupyterLab
RUN conda config --append channels conda-forge
#RUN conda install --quiet --yes 'jupyterlab==1.0.0'
RUN pip install jupyterlab==1.0.0 ipywidgets

RUN conda install -c conda-forge jupyterlab jupyter_conda

RUN jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager --no-build && \
    jupyter labextension install jupyterlab_toastify --no-build && \
    jupyter labextension install jupyterlab_conda --no-build && \
    jupyter labextension install jupyterlab_voyager --no-build && \
    jupyter lab build --dev-build=False --minimize=False && \
    npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

WORKDIR /home/$NB_USER/work/
USER $NB_USER
