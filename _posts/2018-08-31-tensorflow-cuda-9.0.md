---
# layout: single
title: Tensorflow-GPU with CUDA 9.0 on ubuntu 16.04
date: 2018-08-31
permalink: /posts/2018/08/tensorflow-cuda-9.0/
# tags:
#   - cool posts
#   - category1
#   - category2
---

## Introduction

## Getting Started

### **Step 0. Make sure NVIDIA Drivers are uptodate:**

This was a _major headache_ for me, so much so that I'll probably be writing up something specifically about upgrading nvidia drivers "cleanly".

[This guide](https://medium.com/@ikekramer/installing-cuda-8-0-and-cudnn-5-1-on-ubuntu-16-04-6b9f284f6e77)
and others like it were helpful when I was trying to figure it all out.

If you installed cuda toolkit through apt-get or a packaged installer from nvidia uninstall any previous versions
```bash
sudo apt-get purge cuda*
sudo apt-get purge libcudnn6
sudo apt-get purge libcudnn6-dev
```

Download CUDA 9.0, libcudnn, and libnccl deb installers from nvidia
```bash
# CUDA 9.0
wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb

# libcudnn-dev installers
wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libcudnn7-dev_7.0.5.15-1+cuda9.0_amd64.deb

# libnccl-dev installers
wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libnccl2_2.1.4-1+cuda9.0_amd64.deb
wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libnccl-dev_2.1.4-1+cuda9.0_amd64.deb
```

---

**Step 1. Install NVIDIA CUDA:**

Load the libraries into apt-get
```bash
# CUDA 9.0
sudo dpkg -i http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_9.0.176-1_amd64.deb

# libcudnn-dev installers
sudo dpkg -i http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libcudnn7_7.0.5.15-1+cuda9.0_amd64.deb
sudo dpkg -i http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libcudnn7-dev_7.0.5.15-1+cuda9.0_amd64.deb

# libnccl-dev installers
sudo dpkg -i http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libnccl2_2.1.4-1+cuda9.0_amd64.deb
sudo dpkg -i http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/libnccl-dev_2.1.4-1+cuda9.0_amd64.deb

sudo apt-get update
```

Install the library
```bash
sudo apt-get install cuda=9.0.176-1
```

**Step 2. Install NVIDIA cuDNN and nccl:**

```bash
sudo apt-get install libcudnn7-dev libnccl-dev
```

**Step 3. Install Tensorflow:**

In order to compile python binaries you'll need the python-dev.

```bash
$ sudo apt-get install python-dev
```

To use our freshly compiled tensorflow python binaries we'll need to install them with pip.  If you [installed pyenv to manage your python versions](https://jzlab.gitbooks.io/jzkb/content/pyenv.html) you should already have pip as a part of one of your python installations. Just remember to set which python version/environment you want ensorflow-gpu to be installed on.

```bash
$ pyenv local <python version>
```

If you aren't using pyenv \(you probably should be...\), at least make sure you have pip installed and updated.

> This is just one example of installing pip via the package manager but there are lots of ways to do it.

```bash
$ sudo apt-get install python-pip
$ pip install --upgrade pip
```

Finally, use pip to install the source compiled tensorflow package.

```bash
$ pip install tensorflow-gpu==1.8
```

**Step 6. Upgrade protobuf:**

Upgrade to the latest version of the protobuf package:

For Python 2.7:

```bash
$ pip install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/protobuf-3.0.0b2.post2-cp27-none-linux_x86_64.whl
```

For Python 3.4:

```bash
$ pip3 install --upgrade https://storage.googleapis.com/tensorflow/linux/cpu/protobuf-3.0.0b2.post2-cp34-none-linux_x86_64.whl
```

**Step 6. Test your installation:**

To test the installation, open an interactive Python shell:

```bash
$ cd ~/
$ ipython
```

> **NOTE:** Changing directories is important, if you run an interactive shell from where you installed tensorflow it will likely pick up the local version in that directory thereby possibly giving you a false positive result \(like it did to me\)

and import the TensorFlow module:

```python
import tensorflow as tf
print(tf.__version__)

hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```

You should see tensorflow's version which should match the version you checked out from source and “Hello, TensorFlow!”.