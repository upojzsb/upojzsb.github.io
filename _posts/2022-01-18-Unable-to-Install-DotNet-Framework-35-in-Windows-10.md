---
layout:     post
title:      Unable to Install .Net Framework 3.5 in Windows 10
subtitle:   ErrorCode: 0x80071AB1
date:       2022-01-18
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Windows 10
    - Error Code
    - 0x80071AB1
---

# Problem

Unable to install .Net Framework in Windows 10

Error Code is 0x80071AB1

# Solution

Run

```
fsutil resource setautoreset true c:\
```

in PowerShell

[Reference](https://www.reddit.com/r/techsupport/comments/rqpyc8/net_framework_35_including_net_20_and_30_wont/)
