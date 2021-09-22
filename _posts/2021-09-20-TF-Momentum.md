---
layout: post
mathjax: true
title: Trend Following - Momentum
categories: Trading
published: false
---

The momentum indicator is defined as the difference between the current price and the price $N$-days ago. 

$$
M_{t} = C_{t} - C_{t-N}
$$

The rate of change (ROC) is defined as the fraction of the momentum to the price $N$-days ago. 

$$
ROC_t = \frac{M_{t}}{C_{t-N}} = \frac{C_{t} - C_{t-N}}{C_{t-N}}
$$


Using these indicators, we can define a simple strategy: 
* Long: Momentum or ROC is positive
* Short: Momentum or ROC is negative