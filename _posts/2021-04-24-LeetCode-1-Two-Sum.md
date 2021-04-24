---
layout:     post
title:      LeetCode 1. Two Sum
subtitle:   Solution
date:       2021-04-24
author:     UPOJZSB
header-img: img\LeetCode-Logo.png
catalog: true
tags:
    - LeetCode
    - Code
    - Algorithm
    - Easy Problem
---

# Preface

Since I want to improve my ability to develop the valid and fast solution for some problem, I will study computer algorithms from now on. To verify the result of studying, I will solve LeetCode problems. This is my first submission.

# Problem

Easy Problem

[Two sum](https://leetcode.com/problems/two-sum/)

# Solution (Python)

Since We want to find two indices that satisfies ` nums[index1] + nums[index2] == target `, we can maintain two pointers and vary them respectively to find the indices.

```python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        len_nums = len(nums)
        for index1 in range(len_nums):
            for index2 in range(index1+1, len_nums):
                if nums[index1]+nums[index2] == target:
                    return [index1, index2]
```

Complexity:

Time: $ O(N^2) $, Space: $ O(N) $
