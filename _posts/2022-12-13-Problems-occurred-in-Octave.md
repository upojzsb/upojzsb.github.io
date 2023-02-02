---
layout:     post
title:      Problems occurred in Octave
subtitle:   Octave
date:       2022-12-13
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Problem
    - Octave
---

# Prologue

> Sorted by alphabetical order of the name of the packages

# No Response When Plotting
## Situation

When executing commands like `figure`, or `plot(x, y)`, the Octave becomes no response.

It seems that the graphic toolkit works improperly.

## Solution

Execute `graphics_toolkit`, if response is `qt`, try `fltk` by
```MATLAB
graphics_toolkit('fltk')
```

And we can make this change permanent by adding it into `~/.octaverc`


## Reference
[Reference](https://stackoverflow.com/questions/12032494/plot-window-not-responding)

*Updated at 13 Dec, 2022*
