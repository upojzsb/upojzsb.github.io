---
layout:     post
title:      Problems occurred in MATLAB
subtitle:   MATLAB
date:       2023-10-30
author:     UPOJZSB
header-img: img/post-bg-Matlab.png
catalog: true
tags:
    - Problem
    - MATLAB
---

# Prologue

> Sorted by alphabetical order of the name of the function

# Plotting

## Annotation

### The arrow head of annotation is not aligned with the arrow body

Code like this will generate an improper arrow whose arrow line is not aligned with the arrow head.

```MATLAB
hannotation = annotation('arrow');
hannotation.Parent = gca;
hannotation.Color='r';
hannotation.Position = [60, 90, 30, 20];
hannotation.LineWidth = 1;  
```

![Not alligned arrow](/img/post/matlab_problem/annotation_arrow_alignment.png)   

#### Solution

If you don't care about the y-axis direction, add:

```MATLAB
set(gca,'YDir','normal')
```

Or, if the direction of y-axis does matter to you, try to use another implementation of annotation in [Annotation](https://www.mathworks.com/matlabcentral/fileexchange/63760-annotate#functions_tab)

## Reference
[Reference 1](https://www.mathworks.com/matlabcentral/answers/408767-annotation-arrow-head-not-aligned-with-arrow-body#answer_750553)
[Reference 2](https://stackoverflow.com/a/55749028)

*Updated at 30 Oct, 2023*
