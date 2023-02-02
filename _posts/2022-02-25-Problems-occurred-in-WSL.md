---
layout:     post
title:      Problem occurred in WSL
subtitle:   WSL
date:       2022-02-25
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Problem
    - Windows 10
    - WSL
    - WSL2
    - Python
---

# Python

## matplotlib

### UserWarning: Matplotlib is currently using agg, which is a non-GUI backend, so cannot show the figure.

#### Solution
Install python3-tk by:

```sh
sudo apt install python3-tk
```

#### Reference
[Reference](https://stackoverflow.com/questions/56656777/userwarning-matplotlib-is-currently-using-agg-which-is-a-non-gui-backend-so)
