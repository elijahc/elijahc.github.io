---
layout: single
title: Multi-user Jupyterhub
date: 2020-01-11
header:
  teaser: 'jupyter.png'
category: dev
tags:
    - python
toc: true
---

I've started and failed multiple times at hosting jupyterhub in a multi-user environment and I finally got it to work

## Install Jupyterhub

```bash
# npm/node stuff
sudo apt-get -y install npm nodejs
sudo npm install -g configurable-http-proxy

sudo pip install jupyterhub oauthenticator

sudo pip install --upgrade notebook
```

Add Linux system users for everyone who will need access

```bash
sudo adduser neuropil
```

## Running jupyterhub as sudo issues

> See [2] for more details

Since JupyterHub needs to spawn processes as other users, the simplest way is to run it as root, spawning user servers with setuid. But this isn’t especially safe, because you have a process running on the public web as root.

A more prudent way to run the server while preserving functionality is to create a dedicated user with sudo access restricted to launching and monitoring single-user servers.

The sudospawner mediator, the intermediate process, can only do two things:

1. send a signal to another process using the os.kill() call
2. spawn single-user servers

Launching the sudospawner script is the only action that requires a JupyterHub administrator to have sudo access to execute.

### Install [sudospawner](https://github.com/jupyterhub/sudospawner)

#### a. from source

```bash
git clone https://github.com/jupyterhub/sudospawner
cd sudospawner
sudo pip install -e ./
```

> pip install -e ./ installs a cloned git repo into your python env by linking the directory

#### b) using pypi

```bash
sudo pip install sudospawner
```

> by default sudospawner binary is installed at /usr/local/bin/sudospawner

### Create jupyterhub user

```bash
sudo useradd hub -r -s /bin/false
```

> This user shouldn't have a login shell or password (use -r -s /bin/false)

```bash
sudo useradd hub -r
```

### Make Linux usergroup to denote which users hub can spawn notebooks for

Create the group

```bash
sudo groupadd jupyterhub

sudo usermod -a -G jupyterhub hub
```

Add jupyterhub users (students that need access) to this group
```bash
sudo usermod -a -G jupyterhub elijahc

sudo usermod -a -G jupyterhub neuropil
```

Give hub user authority to run sudo spawner for jupyterhub users by editing /etc/sudoers adding the following:
```bash
# the command(s) the Hub can run on behalf of the above users without needing a password
# the exact path may differ, depending on how sudospawner was installed
Cmnd_Alias JUPYTER_CMD = /usr/local/bin/sudospawner

# actually give the Hub user permission to run the above command on behalf
# of the above users without prompting for a password
rhea ALL=(%jupyterhub) NOPASSWD:JUPYTER_CMD
```

#### Test if all the permissions changes worked

This should prompt for your password to switch to rhea, but not prompt for any password for the second switch. It should show some help output about logging options:

```bash
sudo -u rhea sudo -n -u $USER /usr/local/bin/sudospawner --help
Usage: /usr/local/bin/sudospawner [OPTIONS]

Options:

  --help                           show this help information
...
```

To launch jupyterhub as your newly minted hub user use sudo -u

```bash
sudo -u hub jupyterhub -f jupyterhub_config.py
```

### Make jupyterhub its own directory

JupyterHub stores its state in a database, so it needs write access to a directory.
The simplest way to deal with this is to make a directory owned by your Hub user, and use that as the CWD when launching the server.

```bash
sudo mkdir /etc/jupyterhub

sudo chown hub /etc/jupyterhub
```

## Run Jupyterhub over a custom domain with https

### [Buy a domain](domains.google.com)

I use google domains because I wrote a nice [python script](https://gist.github.com/elijahc/8abb89f55a49a1abc9b7dd478db89c06) for doing my own dyndns

{% gist 8abb89f55a49a1abc9b7dd478db89c06 update-dns.py %}

### Create a dyn dns entry for that domain (e.g. hopper.jatlab.org)

### Point the subdomain at your jupyterhub host

```bash
wget https://gist.githubusercontent.com/elijahc/8abb89f55a49a1abc9b7dd478db89c06/raw/d356bbc811e1b140c36edbb4a05a171afa6fff8e/update-dns.py

sudo python update-dns.py
```

your subdomain should now be pointed at the ip address of your jupyterhub host

### Create your own self-signed ssl cert

```bash
cd /etc/jupyterhub

openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mykey.key -out mycert.pem
```

### Edit Jupyterhub configs

Jupyterhub needs to be pointed at these configs to use them and you'll need to update the config to bind to your subdomain (e.g. hopper.jatlab.org) and host on port 443

> will add later

## Configure Jupyterhub to handle authentication using github oauth

> Coming later

## Run Jupyterhub as a system service

> Coming Soon

### References
[1] [Official Instructions](https://github.com/jupyterhub/jupyterhub/wiki/Installation-of-Jupyterhub-on-remote-server)

[2] [sudospawner setup](https://jupyterhub.readthedocs.io/en/stable/reference/config-sudo.html)