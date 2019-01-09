## **colinconwell/deep-notebooks**:fastai-0.7.0-notebook

Dockerfile for building for building the deep-learning framework FastAI for use with jupyter notebook. This docker image can be used with Jupyterhub + DockerSpawner to spawn single-user notebooks.

## Base Notebook
* Ubuntu 16.04
* [CUDA](https://developer.nvidia.com/cuda-toolkit) (GPU version only)
* [cuDNN](https://developer.nvidia.com/cudnn) (GPU version only)
* [iPython/Jupyter Notebook](http://jupyter.org/) (including python2 and python3 kernals)
* [Numpy](http://www.numpy.org/), [SciPy](https://www.scipy.org/), [Pandas](http://pandas.pydata.org/), [Scikit Learn](http://scikit-learn.org/), [Matplotlib](http://matplotlib.org/)
* [OpenCV](http://opencv.org/)

### pull it from dockerhub
```
$ docker pull <dockerhub link>
```

### build it from github repository
```
sudo nvidia-docker build -t <dockerhub link> <github link>
```

# run it as standalone container
nvidia-docker run -it --rm -p 8888:8888 <dockerhub link>

# run it with jupyterhub and Dockerspawner
