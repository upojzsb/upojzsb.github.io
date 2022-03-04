---
layout:     post
title:      Install Madagascar on Windows 10
subtitle:   Madagascar
date:       2022-03-04
author:     UPOJZSB
header-img: img/post-bg-seismic.jpg
catalog: true
tags:
    - Madagascar
    - Windows 10
    - WSL
    - WSL2
---

# Prologue

Madagascar project provides a shared research environment for computational data analysis in geophysics and related fields.

However, official website of madagascar does not provide the guide for installing it on Window via WSL(Windows Subsystem Linux).

This article provides a guide to install madagascar on Windows via WSL2.

# Install
## WSL2 and X Window

See correspondent part in [Install Seismic Unix on Windows 10](https://upojzsb.github.io/2022/02/24/Install-Seismic-Unix-on-Windows-10/).

## Prerequisite packages

```sh
# Git
sudo apt install git

# Basic packages
## I failed to install with this line
# sudo apt install libxaw7-dev freeglut3-dev libnetpbm10-dev libgd-dev libplplot-dev libavcodec-dev libcairo2-dev libjpeg-dev swig python-dev python-numpy g++ gfortran libopenmpi-dev libfftw3-dev libsuitesparse-dev python-epydoc scons git emacs25
## This line of packages had been installed successfully
sudo apt install libxaw7-dev freeglut3-dev libnetpbm10-dev \
libgd-dev libplplot-dev libavcodec-dev \
libcairo2-dev libjpeg-dev swig  \
g++ gfortran libopenmpi-dev \
libfftw3-dev libsuitesparse-dev scons

# Emacs
sudo apt install emacs

# pip
sudo apt install python3-pip

# Python packages
pip install numpy
pip install epydoc
```

## Madagascar

### Prerequisite

Create a directory to save source code of madagascar and download source code from GitHub repository of madagascar

```sh
mkdir ~/madagascar
cd ~/madagascar
git clone https://github.com/ahay/src RSFSRC
```

Make a soft link for python by:
```sh
sudo ln -s /usr/bin/python3 /usr/bin/python
```

### Compile and Install

Go into the directory of the source code of madagascar and run configure by:

```sh
cd ~/madagascar/RSFSRC
./configure --prefix=~/madagascar
```

Make and make install:
```sh
make -j8 # depend on how many cores/CPUs do you have
make install
```

Add `env.sh` file to your profile file like `~/.bashrc` or `~/.zshrc` by add the following sentence to the end of file:

```sh
source ~/madagascar/RSFSRC/env.sh
```

And execute:

```sh
source ~/.zshrc
# or
source ~/.bashrc
```

One last check, execute the following commands to see documents.
```sh
sfin
sfattr
sfspike
sfbandpass
sfwiggle
```

Or visually, execute:

```sh
sfspike n1=1000 k1=300 | sfbandpass fhi=2 phase=y | sfwiggle clip=0.02 title="Welcome to Madagascar" | sfpen
```

# Reference
[Reference 1](https://blog.csdn.net/qq_45317164/article/details/121972078)

[Reference 2](https://www.reproducibility.org/wiki/Installation#Installation_from_source)

[Reference 3](https://www.reproducibility.org/wiki/Windows)
