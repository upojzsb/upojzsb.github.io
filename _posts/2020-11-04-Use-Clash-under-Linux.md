---
layout:     post
title:      在 Linux 下使用 Clash
subtitle:   Use Clash under Linux
date:       2020-10-12
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Linux
    - GFW
    - Clash
---

# 为什么是 Clash

购买的机场服务提供订阅链接的描述为  
> Clash for Linux - [ VMess ]：
> ...

# Clash 在 Linux（Ubuntu） 下的配置

1. 下载 Clash 的二进制文件（或源码，自行编译）

    [某机场提供的二进制文件](https://www.lnds.top/index.php?user/publicLink&fid=859bNNfHj9mz3u7yQBQ9nBuR5u99tU3U7QeouSrrbRF7utSOswMEb5GnpAWMfwU1xFaO0A3TEFPI--bolv3F1c9ThVjgxtBv5yW25K7GMjNvB9UB0A1Q1lHu4jfwzAumw7R-lTduCDjB_Y3t0ytp8ygRmA&file_name=/clash-linux-amd64-v1.2.0.gz)

2. 运行下载的二进制文件，会自动下载一个 Country.mmdb 文件， Clash 会依据此文件进行分流。由于 GFW 的原因，下载可能会出现困难。

    [某机场提供的 Country.mmdb 文件](https://www.lnds.top/index.php?share/file&user=1&sid=rHn6ihRN)

    [backup](/backup/Country.mmdb)

3. 下载机场提供的订阅链接，并改名为config.yaml

4. 将 config.yaml Country.mmdb 放在一个目录下

5. 运行 `/{clash 二进制文件所在路径}/clash -d /{config.yaml、COuntry.mmdb 所在路径}/`，后，将系统代理按照 config.yaml 所示进行修改即可完成

# Script

脚本可以实现 config.yaml 的自动下载和 Clash 的启动，修改链接和目录后设置登录自启动即可。

启动后可以使用 [网页](http://clash.razord.top/) 进行管理。

```
#!/bin/bash

#echo http://clash.razord.top/

wget 此处修改为机场提供的订阅链接 -O config1.yaml --no-proxy

if [ $? -eq 0 ]
then
    rm config.yaml
    mv config1.yaml config.yaml
    echo "`date` successfully download config.yaml" >> log
else
    echo "`date` download failed" >> log
fi

nohup /home/jzsb/clash/clash -d /home/jzsb/clash &
```
（不知道什么原因，代码设置成 shell 在 blog 中的显示会出问题）
# Reference

[Clash For Linux](https://instruction.lnds.top/ssr/linux/clash)
