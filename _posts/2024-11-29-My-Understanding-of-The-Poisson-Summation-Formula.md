---
layout:     post
title:      我对Poisson求和公式的理解
subtitle:   My Understanding of The Poisson Summation Formula
date:       2024-11-29
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Poisson Summation Formula
    - Mathematics
    - Wavelet
---

# 背景

在阅读离散小波变换相关的文章时，遇到诸如 $\mathcal{F}\left( <\phi_{j,k}, \phi_{j,m}> \right)=\sum_{m=-\infty}^{\infty}\left|\Phi\left(\omega+2\pi m\right)\right|^2$ 一类涉及Poisson求和公式的周期求和问题就会产生困惑。前期阅读过 M. Holschneider 所著 *Wavelet: And Analysis Tool* 书中，2.9节对Poisson求和公式有所解释，然而由于阅读时间较久，已然忘却。现重读该部分，并进行记录以便后续查找。

# The Poisson Summation Formula

## 周期化函数

以 $ 2\pi $ 为周期的函数满足 iif $ s(\omega+2\pi) = s(\omega) $ (周期设置为 $ 2\pi $ 是为了方便推广到高维情况，即将一维单位圆推广到高维单位球，对于其它周期的函数，可以通过缩放将其周期化简到$ 2\pi $)。

对于在 $\mathbb{R}$ 上周期为 $2\pi$ 的函数，我们可以将其视为在单位圆 $ \mathbb{T} $ 上 （半径为 $ 1 $ ）。积分可记为：

$$
\int_{\mathbb{T}} \text{d}\omega = \int_{0}^{2\pi}\text{d}\omega.
$$

定义内积：

$$
\left<r,s\right>_\mathbb{T}=\int_{\mathbb{T}} \text{d}\omega\ \bar{r}(\omega)s(\omega) = \int_{0}^{2\pi}\text{d}\omega\ \bar{r}(\omega)s(\omega),
$$

范数：

$$
\lVert s \rVert^2_{L^2(\mathbb{T})} =\left<s,s\right>_\mathbb{T} = \int_{0}^{2\pi}\text{d}\omega\ \left|s(\omega)\right|^2.
$$

如无特殊说明，后文中的“圆”均指单位圆。


## 周期化算子

定义周期化算子 $ \Pi $ 如下，它可以将 $ \mathbb{R} $ 上的非周期函数周期化。

$$
\Pi : L^1\left(\mathbb{R}\right) \to L^1(\mathbb{T}), \quad (\Pi s)(\omega) = \sum_{n\in\mathbb{Z}} s(\omega + 2 \pi n).
$$

此时，定义在 $ \mathbb{R} $ 上的函数 $ r $ 和定义在 $ \mathbb{T} $ 上的函数 $ s $ 的内积可以表示为 $ \left<r, s\right>_\mathbb{R}=\left<\Pi r, s\right>_\mathbb{T}$，证明如下：

$$
\begin{align*}
\left<r, s\right>_\mathbb{R} &= \int_\mathbb{R} \text{d}\omega\ \bar{r}(\omega)s(\omega) \\
    &= \sum_{n\in\mathbb{Z}} \int^{2\pi (n+1)}_{2\pi n}\text{d}\omega\ \bar{r}(\omega) s(\omega) \\
    &= \sum_{n\in\mathbb{Z}} \int^{2\pi}_{0}\text{d}\omega'\ \bar{r}(\omega'+2\pi n) s(\omega'+2\pi n) && \text{(}\omega'=\omega-2\pi n\text{)} \\
    &= \sum_{n\in\mathbb{Z}} \int^{2\pi}_{0}\text{d}\omega\ \bar{r}(\omega+2\pi n) s(\omega) && \text{(}\omega=\omega', s\text{ is with 2}\pi \text{ period.}\text{)} \\
    &=  \int^{2\pi}_{0}\text{d}\omega\ \left(\sum_{n\in\mathbb{Z}}\bar{r}(\omega+2\pi n)\right) s(\omega) \\
    &= \left<\Pi r,s\right>_\mathbb{T}\\
    &\ &&\ && \blacksquare
\end{align*}.
$$

定义等距采样函数：

$$
 \text{└┴┘}_\lambda(\omega) = \sum_{n\in\mathbb{Z}}\delta(\omega-n\lambda).
$$

 使用等距采样函数可将周期化算子表示为：

$$
\Pi_{2\pi}: s\mapsto  \text{└┴┘}_{2\pi} * s.
$$

## 序列和采样

定义整数点取值的函数为序列，记为 $ l(n) $ 或 $ l_n $，其内积表示为：

$$
\left<l, v\right> = \sum_{n\in\mathbb{Z}} \bar{l}(n)v(n),
$$

范数：

$$
\lVert l \rVert^2_{L^2(\mathbb{Z})}.
$$

完美采样算子 $ \Sigma $ 定义为函数从实轴 $ \mathbb{R} $ 到序列 $ \mathbb{Z} $ 的映射：

$$
\Sigma C(\mathbb{R}) \cap L^\infty(\mathbb{R}) \to L^\infty(\mathbb{Z}), \quad s \mapsto \Sigma s, \quad (\Sigma s)(n) = s(n).
$$

可以用周期为1的等距采样函数来表示完美采样算子：

$$
\Sigma: s\mapsto \text{└┴┘}s.
$$

使用其它函数 $ \psi $ 进行采样为非完美采样，定义为：

$$
\Sigma_\psi: L^2(\mathbb{R})\to L^\infty(\mathbb{Z}), \quad (\Sigma_\psi) s(n) = \left<\psi(\cdot-n), s\right>.
$$

## 圆上的Fourier变换

圆上的Fourier变换将圆上的函数映射到序列，定义为：

$$
F^{\mathbb{T}}: L^2(\mathbb{T}) \to L^2(\mathbb{Z}),\quad (F^\mathbb{T}s)(n) = \hat{s} = \left<e_n, s\right> = \int_{\mathbb{T}} \text{d} \omega\ e^{-in\omega}s(\omega).
$$

逆Fourier变换则将序列映射回圆上的函数：

$$
F^{\mathbb{T}\ -1}: L^2(\mathbb{Z}) \to L^2(\mathbb{T}),\quad (F^\mathbb{T\ -1}l)(\omega) = \frac{1}{2\pi}\sum_{n\in\mathbb{Z}} l(n) e^{in\omega}.
$$

当满足某些正则条件时， $ s $ 可以表示为 $ e_n $ 的叠加：

$$
\begin{aligned}
s   &= \frac{1}{2\pi} \sum_{n\in\mathbb{Z}} (F^\mathbb{T}s)(n)e_n \\
    &=  \frac{1}{2\pi} \sum_{n\in\mathbb{Z}} \hat{s}(n)e_n
\end{aligned}.
$$

时域的内积可以在频域计算：

$$
\begin{aligned}
\left<s, r\right>_\mathbb{T}    &= \int_{\mathbb{T}} \text{d}\omega\ \bar{s}(\omega) r(\omega) \\
                                &= \frac{1}{2\pi} \sum_{n\in\mathbb{Z}} \bar{\hat{s}}(n) \hat{r}(n) \\
                                &= \frac{1}{2\pi} \left<\hat{s}, \hat{r}\right>_\mathbb{Z}
\end{aligned}.
$$

Fourier变换同时可以保存能量：
$$
\lVert s \rVert _{L^2(\mathbb{T})}^{2}=\frac{1}{2\pi} \lVert F^{\mathbb{T}}s\rVert _{L^2(\mathbb{Z})}^2.
$$

类似的，定义在整数上的Fourier变换算子及逆算子：

$$
F^\mathbb{Z}: L^2(\mathbb{Z}) \to L^2(\mathbb{T}),\quad (F^\mathbb{Z} l) (\omega)=\sum_{n\in\mathbb{Z}} l(n)e^{-in\omega}, \\
F^{\mathbb{Z}\ -1}: L^2(\mathbb{T}) \to L^2(\mathbb{Z}),\quad s(n)=\frac{1}{2\pi}\int_0^{2\pi} \text(d)\omega\ s(\omega)e^{in\omega}. \\
$$

时域的卷积和频域的乘积在以上两种Fourier算子中等价。

## Poisson求和公式

Poisson求和公式将实轴函数 $s$ 及其周期化后的Fourier变换系数序列联系起来了，具体的：

>***定理9.5.1*** \
>  *在 $ L^1(\mathbb{R}) $ 上，有：*
>  $$
>  \Sigma F= F^\mathbb{T}\Pi,
>  $$
>  *更具体地，对于 $s \in L^2(\mathbb{R})$ ,*
>  $$
>  (F^\mathbb{T}\Pi s)(n)=\hat{s}(n).
>  $$
>  *对于 $s \in L^1(\mathbb{R}), \hat{s} \in L^1(\mathbb{R})$，*
>  $$
>  \sum_{n\in\mathbb{Z}}s(\omega+2\pi n) = \sum_{n\in\mathbb{Z}}\hat{s}(n)e^{in\omega}
>  $$
>  *上式逐点成立。*