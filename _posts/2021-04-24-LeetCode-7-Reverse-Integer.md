---
layout:     post
title:      LeetCode 7. Inverse Integer
subtitle:   Solution
date:       2021-04-24
author:     UPOJZSB
header-img: img/LeetCode-Logo.png
catalog: true
tags:
    - LeetCode
    - Code
    - Algorithm
    - Easy Problem
---

# Problem

Easy Problem

[Inverse Integer](https://leetcode.com/problems/reverse-integer/)

# Solution (Python)

## 2021-04-24

Since we want to reverse the integer, an easy way to achieve is to transform integer into string, reverse the string and transform back.

```python
class Solution(object):
    def reverse(self, x):
        """
        :type x: int
        :rtype: int
        """
        sign = 1 if x > 0 else -1
        str_x = str(x*sign) # Make x positive
        str_x = str_x[::-1]
        ret_x = int(str_x)*sign

        if -2**31 <= ret_x <= 2**31 -1:
            return ret_x
        return 0

```

Complexity:

Time: $ O(N) $, Space: $ O(N) $
