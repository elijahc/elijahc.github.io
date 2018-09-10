---
layout: single
title: Using Jupyterhub with pyenv
excerpt: I use jupyterhub a lot for data exploration and this is my setup for running it remotely
category: dev
tags:
    - python
---

I use jupyterhub a lot for data exploration and this is my setup for running from a remote workstation so I can access it in a browser from anywhere

# Using JupyterHub

## Installation Instructions

> Python 3.4 or greater

https://github.com/jupyterhub/jupyterhub

```bash
pip install --upgrade jupyterhub notebook

```

## Configure everything

## Run the server

```bash
# With ssl
$ jupyterhub --ip 10.0.1.2 --port 443 --ssl-key my_ssl.key --ssl-cert my_ssl.cert

# Without ssl
$ jupyterhub --ip 10.0.1.2 --port 8000
```

## Multiple pyenv/venv kernels

Ref: https://www.alfredo.motta.name/create-isolated-jupyter-ipython-kernels-with-pyenv-and-virtualenv/

### Install pyenv
See kb article on pyenv

### Install ipykernel on the env you need

```bash
$ pyenv local my_venv

$ pip install ipykernel
```


```bash
# Confirm jupyter is installed correctly
$ pip list | grep jupyter
jupyter-client (4.1.1)
jupyter-core (4.0.6)
```

### Find where Jupyter looks for kernel configs

```bash
$ jupyter --paths
config:
    /home/elijahc/.jupyter
    /usr/local/etc/jupyter
    /etc/jupyter
data:
    /home/elijahc/.local/share/jupyter
    /usr/local/share/jupyter
    /usr/share/jupyter
runtime:
    /run/user/1000/jupyter
```
For global kernels for all users store them at /usr/local/share/jupyter or /usr/share/jupyter

Make a new directory for the new kernel
```bash
$ sudo mkdir -p /usr/local/share/jupyter/kernels/my_venv
```

Add the new kernel.json file
```bash
$ sudo touch /usr/local/share/jupyter/kernels/my_venv/kernel.json
```

kernel.json
```json
$ cat kernel.json
{
 "argv": [ "/home/elijahc/.pyenv/versions/my_venv/bin/python", "-m", "ipykernel",
          "-f", "{connection_file}"],
 "display_name": "my_venv",
 "language": "python"
}
```

## Setup Jupyterhub as a linux service
Ref: https://github.com/jupyterhub/jupyterhub/wiki/Run-jupyterhub-as-a-system-service
