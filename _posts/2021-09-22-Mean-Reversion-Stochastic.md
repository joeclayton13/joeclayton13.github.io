---
layout: post
mathjax: true
title: Stochastic Oscillator
categories: trading
tags: python 
published: false
---

The stochastic oscillator measures the location of the close price relative to the difference between the $N$-day highest high ($HH_{t,N}$) and lowest low ($LL_{t,N}$). The fast stochastic oscillator takes two components: $\text{Fast } % K_{t,N}$ and $\text{Fast } % D_{t,N}$. These are given by,

$$
\text{Fast } % K_{t,N} = \frac{C_t - LL_{t,N}}{HH_{t,N} - LL_{t,N}} \times 100 
$$

$$
\text{Fast } % D_{t,N} = \frac{1}{n} \sum_{i = 0}^{n-1} %K_{t-i, N}
$$

Both of these quantites take values in the interval $x \in [0,100]$. A value $x > 80$ indicates overbought conditions, and values $x < 20$ indicate underbought conditions. 