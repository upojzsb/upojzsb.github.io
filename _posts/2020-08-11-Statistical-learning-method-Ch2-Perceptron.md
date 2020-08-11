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

假设输入空间 $ X \subseteq R^n $ ，输出空间 $ Y=\left{ +1, -1 \right} $ ，输入 $ x \in X $ 为输入特征的向量，输出 $ y \in Y $ 为输入向量的分类，则函数

$$
f(x)=sign( \omega \cdot x+b)
$$

称为感知机，$ \omega \subseteq R^n $ 和 $ b \subseteq R $ 是感知机模型的参数，它们分别代表权重（权值）和偏置。sign为符号函数，定义为：

$$
sign(x) =
\begin{cases}
+1, & x \ge 0 \\
-1, & x < 0
\end{cases}
$$

# 几何解释

感知机在训练完成后得到方程

$$
\omega\cdot x +b = 0
$$

对应在 $ R^n $ 空间中的超平面，其中 $ \omega $ 为超平面的法向量， $ b $ 为截距。该超平面将空间分为两部分，位于不同部分的样本分别为正例和负例。

# 学习策略

## 线性可分性

对于数据集

$$
T={(x_1, y_1), ..., (x_N, y_N)}
$$

其中 $ x \subseteq R^n $ ，$ y \subseteq \left{ +1, -1 \right} $ ，如果存在超平面S

$$
\omega\cdot x + b = 0
$$

可以将数据集中的正例和负例完全正确的区分，则称该数据集线性可分。

## 感知机学习策略

假设数据集线性可分，那么感知机的目的就是寻找可以将数据集中的正例负例完全正确区分的超平面S。我们可以定义一个损失函数作为学习策略，表示感知机分类错误的严重情况，然后最小化这个损失函数。

一个自然的想法是将损失函数定义为误分类的数量，但是这个函数并不关于 $ \omega $ 和 $ b $ 可导，难以优化。

另一个想法是将误分类的样本到超平面距离的总和定义为损失函数，这种方式是感知机所采用的。

首先定义样本到超平面的距离。已知 $ \omega $ 是超平面的法向量，假设 $ P(x) $ 为超平面上一点，$ Q(x_0) $为样本点，则PQ的距离

$$

\begin{align}
d =& |\frac{\vec{PQ}\cdot\omega}{||\omega||_{2}}| \\
=& \frac{\omega}{||\omega||_{2}}|x_0-x| \\
=&\frac{1}{||\omega||_{2}}|\omega\cdot x_0 - \omega \cdot x| \\
=& \frac{1}{||\omega||_{2}}|\omega\cdot x_0 + b|
\end{align}

$$

所以，对于误分类数据 $ (x_i, y_i) $ 而言，可以定义其到超平面的距离：

$$
\frac{1}{||\omega||_{2}}y_{i}(\omega\cdot x_{i} + b) > 0
$$

由于 $ \frac{1}{\|\|\omega\|\|_2} $仅对数据进行等比例缩放，所以不予考虑。设 $ M $ 为误分类集合，最终得到损失函数为：

$$
L(\omega, b) = -\sum_{x_i\in M}y_i(\omega\cdot x_i + b)
$$

# 学习算法

## 原始形式

对于数据集

$$
T={(x_1, y_1), ..., (x_N, y_N)}
$$

感知机算法就是对最小化问题

$$
\min_{\omega, b} L(\omega, b) = -\sum_{x_i\in M}y_i(\omega\cdot x_i + b)
$$

的求解。


## 对偶形式
$1 2 3 \{1 \}$
