---
layout:     post
title:      统计学习方法读书笔记 Ch4 朴素贝叶斯法（Naive Bayes）
subtitle:   Statistical learning method study notes Ch4. Naive Bayes
date:       2020-08-18
author:     UPOJZSB
header-img: img/slm-v2.jpg
catalog: true
tags:
    - Statistical learning method
    - Study notes
    - Naive Bayes
---

# 模型

朴素贝叶斯算法是一种基于贝叶斯定理和特征条件独立假设的分类算法。其接受待分类实例的特征向量作为输入，对该实例分类的结果作为输出。具体定义如下：

存在随机变量 $ X, Y $ 构成联合概率分布 $ P(X, Y) $ 可以生成训练集

$$

T = \lbrace (x_1, y_1), ..., (x_N, y_N) \rbrace

$$

其中 $ x \in R^n , y_i \in \lbrace c_1, ..., c_K \rbrace $，$ K $ 为分类的数量。

朴素贝叶斯就是对联合概率分布 $ P(X,Y) $ 的学习。具体的，可以根据先验概率

$$

P(Y=c_k)

$$

及条件概率

$$

P(X=x|Y=c_k) = P(X^{(1)}=x^{(1)}, ..., X^{(n)}=x^{(n)}|Y=c_k)

$$

学习联合分布概率 $ P(X, Y) $。

如果对所有 $ x \in  R^n $ 建模，则该模型的参数量会随着 $ x $ 的复杂程度指数增长，所以，一般会对模型进行简化，即进行独立条件假设：

将

$$

\begin{align}
P(X=x|Y=c_k) =& P(X^{(1)}=x^{(1)}, ..., X^{(n)}=x^{(n)}|Y=c_k) \\
=& P(X^{(1)}=x^{(1)}|X^{(2)}=x^{(2)}, ..., X^{(n)}=x^{(n)},Y=c_k)\cdot \\
& ... \\
& P(X^{(n-1)}=x^{(n-1)}|X^{(n)}=x^{(n)},Y=c_k) \cdot \\
& P(X^{(n)}=x^{(n)}|Y=c_k)
\end{align}

$$

简化为

$$

\begin{align}
P(X=x|Y=c_k) =& P(X^{(1)}=x^{(1)}, ..., X^{(n)}=x^{(n)}|Y=c_k) \\
=& \prod_{i=1}^{n}P(X^{(i)}=x^{(i)}|Y=c_k)
\end{align}

$$

独立性假设认为 $ X^{(i)} $ 和 $ X^{(j)} (i \ne j)$ 之间无关，这会简化朴素贝叶斯，但同样会牺牲一定的准确率（例如在垃圾邮件分类中，词语“安全账户”和“警告”同时出现的概率与高于两词分别出现）。

在学习了联合概率分布后，当输入一个新实例时 $ x \in R^n $ ，会对其后验概率进行计算：

$$

\begin{align}
P(Y=c_k|X=x) =& \frac{P(X=x|Y=c_k) \cdot P(Y=c_k)}{P(X=x)} \\
=& \frac{P(X=x|Y=c_k) \cdot P(Y=c_k)}
        {\sum_{k=1}^{K}P(X=x|Y=c_k)P(Y=c_k)} \\
=& \frac{\prod_{i=1}^{n}P(X^{(i)}=x^{(i)}|Y=c_k) \cdot P(Y=c_k)}
        {\sum_{k=1}^{K}\Bigg(\prod_{i=1}^{n}P(X^{(i)}=x^{(i)}|Y=c_k)P(Y=c_k)\Bigg)} \\
\end{align}

$$

由于对于任意实例，

$$

\sum_{k=1}^{K}\Bigg(\prod_{i=1}^{n}P(X^{(i)}=x^{(i)}|Y=c_k)P(Y=c_k)\Bigg)

$$

为常数，所以朴素贝叶斯分类器表示为：

$$

y = f(x) = \arg \max_{c_k} \prod_{i=1}^{n}P(X^{(i)}=x^{(i)}|Y=c_k) \cdot P(Y=c_k)

$$

# 参数估计

先验概率 $ P(Y=c_k) $ 实质上是样本集 $ T $ 中，分类为 $ c_k $ 的样本的占比，即：

$$

P(Y=c_k) = \frac{\sum_{i=1}^{N}I(y_i = c_k)}{N}

$$

设 $ a \in S $ 为实例特征向量的取值范围，条件概率 $ P(X^{(i)}=a\|Y=c_k) $ 实质上是样本集 $ T $ 中，分类为 $c_k $ 且 $ X^{(i)} $ 取 $ a $ 的样本数量占分类为 $ c_k $的比例，即：

$$

P(X^{(i)}=a\|Y=c_k) = \frac{\sum_{j=1}^{N}I(X^{(i)} = a , y_j = c_k)}{\sum_{i=1}^{N}I(y_i = c_k )}

$$

# 拉普拉斯平滑

利用贝叶斯对样本进行估计时，可能会出现概率为0的情况，使分类出现偏差。

在计算概率时，可以在分子加一，并在分母加分类数量，来避免这种情况。这种方式叫拉普拉斯平滑。
