---
layout:     post
title:      统计学习方法读书笔记 Ch2 感知机（Perceptron）
subtitle:   Statistical learning method study notes Ch2. Perceptron
date:       2020-08-11
author:     UPOJZSB
header-img: img/slm-v2.jpg
catalog: true
tags:
    - Statistical learning method
    - Study notes
    - Perceptron
---

# 模型

感知机是一种二分类的线性模型，其输入为实力特征的向量，输出分类结果。具体定义如下：

假设输入空间 $ X \belongto R^n $ ，输出空间 $ Y={+1, -1} $ ，输入 $ x \belongto X $ 为输入特征的向量，输出 $ y \belongto Y $ 为输入向量的分类，则函数

$$
f(x)=sign( \omega \dot x+b)
$$

称为感知机，\omega \belongto R^n 和 b \belongto R 是感知机模型的参数，它们分别代表权重（权值）和偏置。sign为符号函数，定义为：

$$$
$$$
