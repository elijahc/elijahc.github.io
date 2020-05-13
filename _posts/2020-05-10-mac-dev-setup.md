---
layout: single
title: New Mac Dev Setup
date: 2020-05-10
excerpt: Every few years when I have to upgrade my dev laptop getting it setup for development is usually an endeavor.
header:
  teaser: 'coding-computer-data-depth-of-field.jpg'
  caption: "Photo by Kevin Ku from [**Pexels**](https://www.pexels.com/photo/coding-computer-data-depth-of-field-577585/)"
  overlay_image: 'coding-computer-data-depth-of-field.jpg'
  overlay_filter: 0.5
  actions:
  - label: "Github"
    url: "https://github.com/elijahc/mac-dev-setup"
category: dev
tags:
- python
- ruby
---

# mac-dev-setup
Every few years when I have to upgrade my MacBook getting it setup for development is usually an endeavor.
I created this to help me install dev essentials or as a helpful starting place for others.

In a previous life I was a full-stack web developer (primarily ruby) but now I do more datascience work (Python and R). This spec reflects that mixed background.

I took lots of inspiration from:
- [nicolashery/mac-dev-setup](https://github.com/nicolashery/mac-dev-setup)
- [Tania Rascia](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/)

# Getting Started

Configure the laptop according to your preferences, make you sure you are up-to-date on security patches

Clone this repository
```bash
mkdir ~/src
cd src
git clone https://github.com/elijahc/mac-dev-setup
```

## Homebrew

Install the [Homebrew package manager](https://brew.sh/). This will allow you to install almost any app from the command line.

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install homebrew applications
```bash
cd ~/src/mac-dev-setup  && ./brew.sh
```

## Git

I use Git/Github (obviously) as my Version Control System (VCS)

`brew.sh` should have installed git for you, now it just needs to be configured

```bash
touch ~/.gitconfig
```

Update gitconfig
```ini
[user]
  name   = Firstname Lastname
  email  = you@example.com
[github]
  user   = username
[alias]
  a      = add
  cm     = commit -m
  s      = status
```

With the above aliases, I can run `git s` instead of `git status`, for example. The less I have to type, the happier I am.

## SSH

I often have to shell into remote systems for heavier duty work.

```bash
mkdir ~/.ssh && touch ~/.ssh/config
```

update `.ssh/config`
```ini
Host *
  AddKeysToAgent yes
  UseKeychain yes
  ForwardAgent yes

Host myssh
  HostName example.com
  User user
```

Create and RSA key:
```bash
ssh-keygen -t rsa -b 4096 -C "email@example.com"
```

Copy it to any hosts added to `~/.ssh/config`:
```bash
ssh-copy-id myssh
```

## Vim

I keep heavily customized configs for Vim in separate dotfiles repo.

Otherwise, Tim Pope as a set of sensible vim defaults.

```bash
mkdir -p ~/.vim/pack/tpope/start
cd ~/.vim/pack/tpope/start
git clone https://tpope.io/vim/sensible.git
```

## Shell

I like zsh. Catalina comes installed with zsh, but it can be installed with brew if necessary:

```bash
brew install zsh zsh-completions
```

Oh My Zsh has sensible defaults. Install it with:
```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Install zsh-syntax-highlighting:
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

I like Avid theme

# Programming Languages

## Python

macOS, like Linux, ships with Python already installed. I like using `pyenv` to manage multiple versions of Python (ex: 2.7 and 3) should we need to.


Install pyenv via Homebrew by running:
```bash
brew install pyenv pyenv-virtualenv
```

Add the following to `.bash_profile`:
```bash
# Load pyenv and virtual env
if command -v pyenv 1>/dev/null 2>&1; then eval "$(pyenv init -)"; fi
if which pyenv-virtualenv-init > /dev/null; then eval "$(pyenv virtualenv-init -)"; fi
```

## Ruby

Install rbenv
```bash
brew install rbenv
```

Add the following to `.bash_profile`/`.bashrc`:
```bash
eval "$(rbenv init -)"
```

## Node
Use Node Version Manager (nvm) to install Node.js. This allows you to easily switch between Node versions, which is essential.

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.2/install.sh | bash
```

Install the latest version:
```bash
nvm install node
```

Add the following to your `.bash_profile`:
```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```