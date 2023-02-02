---
layout:     post
title:      Install Seismic Unix on Windows 10
subtitle:   Seismic Unix
date:       2022-02-24
author:     UPOJZSB
header-img: img/post-bg-seismic.jpg
catalog: true
tags:
    - Installation
    - Seismic Unix
    - SU
    - Windows 10
    - WSL
    - WSL2
---

# Prologue

Seismic Unix (SU) is a series of programs used to process seismic data. It's originally launched on *nix OS's, however, Microsoft have launched WSL (Windows subsystem Linux) which allows us to run Linux programs on Windows.

This article records how I install SU on WSL2.

# Install
## WSL2

Open Control Panel, find Turn Windows Features On or Off, and enable the Windows Subsystem for Linux. Restart is needed after OK has been clicked.

After reboot, open Windows Store, login your Microsoft account and install Ubuntu.

## X Window

In order to run some programs which will display something via X.org, a X window server will be needed. Here we install [VcXsrv](https://sourceforge.net/projects/vcxsrv/) as our X server.

VcXsrv should be started **before** starting WSL environment.

Launch `XLaunch` to config VcXsrv. It's worth to notice that `Disable access control` should be **enabled**, otherwise `Authorization required, but no authorization protocol specified` error will occur.  

To send graphics to VcXsrv, some modification should be made under WSL. Add following command to the file like `.zshrc` or `.bashrc`

```sh
export hostip=$(cat /etc/resolv.conf |grep -oP '(?<=nameserver\ ).*')
export DISPLAY=$hostip:0
```

If you use WSL1, use following command instead:

```sh
export DISPLAY=:0
```

After finished steps above, restart your WSL by the following command under PowerShell:

```powershell
wsl --shutdown
```

Then, install `x11-apps` and run `xeyes` to test if steps above is correctly finished.

```sh
sudo apt install x11-apps
xeyes &
```

A window with a pair of eyes should occur.

## SU

Set some environment variables. Add following commands to your profile like `.zshrc` or `.bashrc`.

```sh
export CWPROOT=$HOME
export PATH=$PATH:$HOME/bin
```

Get SU files from [Seismic-unix.org](https://wiki.seismic-unix.org/doku.php). Here we download `cwp_su_all_44R23.tgz`.

Copy the file to `$HOME` and unzip it by:

```sh
mv /mnt/c/Users/lzhao/Downloads/cwp_su_all_44R23.tgz ~
tar xvf cwp_su_all_44R23.tgz
```

Use the correct `Makefile.config` file by

```sh
cd src
mv Makefile.config Makefile.config.old
cp ./configs/Makefile.config_Linux_x86_64 ./Makefile.config
```

Install required development packages by:

```sh
sudo apt install build-essential
sudo apt install gfortran
sudo apt install libx11-dev
sudo apt install libxt-dev
sudo apt install freeglut3-dev
sudo apt install libxmu-dev libxi-dev
sudo apt install libc6
sudo apt install libuil4
sudo apt install x11proto-print-dev
sudo apt install libmotif-dev
```

There is some mistake in the Makefile, correct it by:

```sh
sed -i 's/44R18/44R23/g' Makefile
```

Install SU by:

```sh
make install
make xtinstall
make finstall

# Optional
make mglinstall
make utils
make xminstall
make sfinstall
```

One last check:

```sh
suplane | suximage title="test"
```

# Reference

[Reference 1](https://suwindows10.blogspot.com/2016/08/how-to-install-seismic-unix-su-on.html)

[Reference 2](https://superuser.com/questions/1174563/when-trying-to-connect-remote-clients-to-cygwin-x-i-get-authorization-required)
