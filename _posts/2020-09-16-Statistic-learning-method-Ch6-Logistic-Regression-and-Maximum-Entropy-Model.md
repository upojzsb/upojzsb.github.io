---
layout:     post
title:      统计学习方法读书笔记 Ch6 逻辑斯蒂回归与最大熵模型（Logistic Regression and Maximum Entropy Model）
subtitle:   Statistical learning method study notes Ch6. Logistic Regression and Maximum Entropy Model
date:       2020-09-16
author:     UPOJZSB
header-img: img/slm-v2.jpg
catalog: true
tags:
    - Statistical learning method
    - Study notes
    - Logistic Regression
    - Maximum Entropy Model
---

# Logistic 模型

Logistic 回归是一种分类模型，它针对于服从 Logistic 分布的随机变量进行建模。

## Logistic 分布

Logistic 分布的分布函数和概率密度函数如下：

$$

P(X \le x) = \frac{1}{1+e^{-(x-\mu)/\gamma}} \\

p(x) = \frac{e^{-(x-\mu)/\gamma}}{\gamma (1+e^{-(x-\mu)/\gamma})^2}

$$

式中，$ \mu $ 为位置参数， $ \gamma \gt 0 $ 为形状参数。

Logistic 分布的概率密度函数为钟形曲线，分布函数为 S 形曲线 ( sigmoid )。

## 二项 Logistic 回归

### 模型

二项 Logistic 回归可以对 $ P(Y\|X) $ 建模，其中 $ X $ 为输入特征， $ Y $ 为分类结果，具体模型为：

$$

\begin{align}

P(Y=1|x) =& \frac{\exp(\omega \cdot x + b)}{1+\exp(\omega \cdot x + b)} \\

P(Y=0|x) =& \frac{1}{1+\exp(\omega \cdot x + b)}

\end{align}

$$

其中， 输入特征 $ x \in R^n $，输出 $ y \in \lbrace 0, 1 \rbrace $，参数包括 $ \omega \in R^n $ 和 $ b \in R $。

为了方便起见，重新定义 $ \omega = (\omega^{(1)}, ..., \omega^{(n)}, b)^T $ 和 $ x = (x^{(1)}, ..., x^{(n)}, 1)^T $ ，此时 $ \omega \cdot x + b $ 可以改写为 $ \omega \cdot x $ 。

考虑 Logistic 回归模型的输入和输出，不难发现，它将实数域内的的值 $ \omega \cdot x $ 转换为概率 $ P(Y=1\|x) = \frac{\exp(\omega \cdot x)}{1+\exp(\omega \cdot x)}  $，即输入越接近 $ +\infty $ 概率越接近1，输入越接近 $ -\infty $ 概率越接近0。

### 参数估计

针对训练集

$$

T = \lbrace (x_1, y_1), ..., (x_N, y_N) \rbrace

$$

可以利用极大似然估计法对模型的参数进行估计。

构建对数似然函数：

$$

\begin{align}

L(\omega) =& \log \prod_{i=1}^{N} (P(Y=1|x))^{y_i} (P(Y=0|x))^{1-y_i} \\

=& \sum_{i=1}^N \big( y_i \log P(Y=1|x) + (1-y_i)P(Y=0|x) \big) \\

=& \sum_{i=1}^{N} \big(  y_i(\omega \cdot x_i) - \log (1|\exp (\omega \cdot x_i)) \big)

\end{align}

$$

最大化 $ L(\omega) $ 就可以得到 $ \omega $ 的估计值。

## 多项 Logistic 回归

假设随机变量 $ Y $ 的取值范围为 \lbrace 1, ..., K \rbrace，则多项 Logistic 回归模型为：

$$

\begin{align}

P(Y=k|x) =& \frac{\exp(\omega_k\cdot x)}{1+\sum_{i=1}^{K-1}\exp (\omega_i\cdot x)}, k=1, ..., K-1 \\

P(Y=K|x) =& \frac{1}{1+\sum_{i=1}^{K-1}\exp (\omega_i\cdot x)}

\end{align}

$$

# 最大熵模型

最大熵模型认为，在所有可能的概率模型中，熵最大的模型是最好的模型。
