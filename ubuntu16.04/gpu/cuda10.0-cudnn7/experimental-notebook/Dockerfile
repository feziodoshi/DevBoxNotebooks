# cocoflow/experimental:latest

# > sudo nvidia-docker build -t cocoflow/experimental:latest https://github.com/ColinConwell/Notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/experimental-notebook

FROM cocoflow/crockpot:latest

#MAINTAINER ColinConwell <colinconwell@gmail.com, conwell@g.harvard.edu>

USER root
WORKDIR $HOME

# ==============================================================
#  Install Jupyterlab Extensions
# ==============================================================

RUN pip install jupyterlab-git jupyterlab_latex \
    jupyterlab_code_formatter ipympl

RUN jupyter labextension install @jupyterlab/plotly-extension --no-build && \
  jupyter labextension install jupyterlab-drawio --no-build && \
  jupyter labextension install jupyterlab-scales --no-build && \
  jupyter labextension install jupyterlab_bokeh --no-build && \
  jupyter labextension install @jupyterlab/latex --no-build && \
  jupyter labextension install @jupyterlab/toc --no-build && \
  jupyter labextension install @jupyterlab/git --no-build && \
  jupyter labextension install @ryantam626/jupyterlab_code_formatter && \
  jupyter labextension install @krassowski/jupyterlab_go_to_definition && \
  #jupyter labextension install jupyterlab_voyager --no-build && \
  jupyter labextension install jupyter-matplotlib && \
    jupyter lab build --dev-build=False && \
    npm cache clean --force && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    rm -rf /home/$NB_USER/.cache/yarn && \
    rm -rf /home/$NB_USER/.node-gyp && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# RUN git clone https://github.com/ColinConwell/jupyterlab-variableInspector \
#  && cd jupyterlab-variableInspector && npm install \
#  && npm run build && jupyter labextension install .

# ==============================================================
#  Install Julia
# ==============================================================

ENV JULIA_DEPOT_PATH=/opt/julia
ENV JULIA_PKGDIR=/opt/julia
ENV JULIA_VERSION=1.2.0

RUN mkdir /opt/julia-${JULIA_VERSION} && \
    cd /tmp && \
    wget -q https://julialang-s3.julialang.org/bin/linux/x64/`echo ${JULIA_VERSION} | cut -d. -f 1,2`/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    echo "926ced5dec5d726ed0d2919e849ff084a320882fb67ab048385849f9483afc47 *julia-${JULIA_VERSION}-linux-x86_64.tar.gz" | sha256sum -c - && \
    tar xzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz -C /opt/julia-${JULIA_VERSION} --strip-components=1 && \
    rm /tmp/julia-${JULIA_VERSION}-linux-x86_64.tar.gz
RUN ln -fs /opt/julia-*/bin/julia /usr/local/bin/julia

# Show Julia where conda libraries are \
RUN mkdir /etc/julia && \
    echo "push!(Libdl.DL_LOAD_PATH, \"$CONDA_DIR/lib\")" >> /etc/julia/juliarc.jl && \
    # Create JULIA_PKGDIR \
    mkdir $JULIA_PKGDIR && \
    chown $NB_USER $JULIA_PKGDIR && \
    fix-permissions $JULIA_PKGDIR

USER $NB_UID

RUN julia -e 'import Pkg; Pkg.update()' && \
  (test $TEST_ONLY_BUILD || julia -e 'import Pkg; Pkg.add("HDF5")') && \
  julia -e "using Pkg; pkg\"add IJulia\"; pkg\"precompile\"" && \ 
  # move kernelspec out of home \
  mv $HOME/.local/share/jupyter/kernels/julia* $CONDA_DIR/share/jupyter/kernels/ && \
  chmod -R go+rx $CONDA_DIR/share/jupyter && \
  rm -rf $HOME/.local && \
  fix-permissions $JULIA_PKGDIR $CONDA_DIR/share/jupyter

USER root

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
