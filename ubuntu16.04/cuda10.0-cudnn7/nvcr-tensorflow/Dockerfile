# sudo docker build -t cocoflow/nvcr-tensorflow:ubuntu16.04-cuda10.0-cudnn7 https://github.com/ColinConwell/notebooks.git#master:ubuntu16.04/gpu/cuda10.0-cudnn7/nvcr-tensorflow

FROM nvcr.io/nvidia/tensorflow:19.09-py3

ARG NB_USER="jovyan"
ARG NB_UID="1000"
ARG NB_GID="100"

USER root

RUN pip install jupyterhub

RUN apt-get clean && apt-get update && apt-get install -y locales && \
	echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && \
	locale-gen

ENV SHELL=/bin/bash \
	NB_USER=jovyan \
	NB_UID=1000 \
	NB_GID=100 \
	LC_ALL=en_US.UTF-8 \
	LANG=en_US.UTF-8 \
	LANGUAGE=en_US.UTF-8 \
	JUPYTER_ENABLE_LAB=1

ENV HOME=/home/$NB_USER

ADD fix-permissions /usr/local/bin/fix-permissions
# RUN chmod u+x /usr/local/bin/fix-permissions
# Create jovyan user with UID=1000 and in the 'users' group
# and make sure these dirs are writable by the `users` group.
RUN groupadd wheel -g 11 && \
	echo "auth required pam_wheel.so use_uid" >> /etc/pam.d/su && \
	useradd -m -s /bin/bash -N -u $NB_UID $NB_USER && \
	chmod g+w /etc/passwd && \
	fix-permissions $HOME

# Setup work directory
USER root

RUN mkdir /home/$NB_USER/work && \
	fix-permissions /home/$NB_USER

WORKDIR $HOME/work

# Add Tini
ENV TINI_VERSION v0.18.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /tini
RUN chmod +x /tini
ENTRYPOINT ["/tini", "-g", "--"]
CMD ["start-notebook.sh"]

# Add local files as late as possible to avoid cache busting
COPY credential-helper.sh /usr/local/bin/
COPY start.sh /usr/local/bin/
COPY start-notebook.sh /usr/local/bin/
COPY start-singleuser.sh /usr/local/bin/
COPY jupyter_notebook_config.py /etc/jupyter/
COPY genpass.py .
RUN fix-permissions /etc/jupyter/

WORKDIR $HOME/work

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_UID
