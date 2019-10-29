## **colinconwell/deep-notebooks**:some-notebook

Dockerfile for building for building the deep-learning framework <framework link> for use with jupyter notebook. This docker image can be used with Jupyterhub + DockerSpawner to spawn single-user notebooks.

### pull it from dockerhub
```
$ docker pull <dockerhub link>
```

### build it from github repository
```
sudo nvidia-docker build -t <dockerhub link> <github link>
```
