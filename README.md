## ColinConwell/Notebooks

*...This is a work in progress!*

A collection of Dockerfiles for building docker containers used to
spawn single-user notebooks with DockerSpawner in JupyterHub. All notebooks are GPU enabled (with CUDA + cuDNN), and there are different containers supporting several popular deep-learning frameworks (e.g., Tensorflow, Torch, Caffe, etc.).

The base notebook was created by modifying nvidia-cuda-devel to work as a single-user notebook with jupyterhub, and then adding additional packages.

Most of the framework notebooks (e.g., Tensorflow, Torch, etc.) were created by copying  https://github.com/floydhub/dl-docker, and then making minor modifications for compatibility with the base-notebook.

## Base Notebook
* [Ubuntu](https://www.ubuntu.com/)
* [CUDA](https://developer.nvidia.com/cuda-toolkit) (GPU version only)
* [cuDNN](https://developer.nvidia.com/cudnn) (GPU version only)
* [iPython/Jupyter Notebook](http://jupyter.org/) (including python2 and python3 kernals)
* [Numpy](http://www.numpy.org/), [SciPy](https://www.scipy.org/), [Pandas](http://pandas.pydata.org/), [Scikit Learn](http://scikit-learn.org/), [Matplotlib](http://matplotlib.org/)
* [OpenCV](http://opencv.org/)

## Frameworks
https://developer.nvidia.com/deep-learning-frameworks
* [Pytorch](http://pytorch.org/)
* [Tensorflow](https://www.tensorflow.org/), with the option to use [Keras](http://keras.io/) or [TFLearn](http://tflearn.org/)
* [Torch](http://torch.ch/) (includes nn, cutorch, cunn and cuDNN bindings), and [iPython/Jupyter Notebook](http://jupyter.org/) with the iTorch kernel.
* [CNTK v2.0-GPU-1bit-SGD](https://www.microsoft.com/en-us/cognitive-toolkit/)
* [NVCaffe](https://github.com/NVIDIA/caffe.git) (NVIDIA's fork of Caffe)
* [Caffe](http://caffe.berkeleyvision.org/)

## Frameworks to Be Added
* [Caffe2](https://caffe2.ai/)
* [Chainer](https://chainer.org/)
* [MXNET](http://mxnet.io/)
* [Digits](https://developer.nvidia.com/digits)
* [Theano](http://deeplearning.net/software/theano/) which can be used with or without [Keras](http://keras.io/) or [Lasagne](http://lasagne.readthedocs.io/en/latest/)

## Build

## Configure Jupyterhub
