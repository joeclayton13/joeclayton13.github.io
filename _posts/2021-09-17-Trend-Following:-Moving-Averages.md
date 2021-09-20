---
layout: post
mathjax: true
title: Trend Following - Moving Average Crossover
categories: Trading
---

The basic idea is: 

* Long: if the close price is above the $N$-day moving average.
* Short: if the close price is below the $N$-day moving average.

A problem with the outline above is that single-day stock prices can be noisy. It's possible that the market may not have a clear trend so the price is constantly fluctuating above and below the $N$-day moving average. To reduce noise, we can use the $N$ and $M$-day moving averages ($N > M > 1$). The idea is, 

* Long: if the $M$-day moving average crosses above the $N$-day moving average.
* Short: if the $M$-day moving average crosses below the $N$-day moving average.

I considered 50 and 200 day moving averages. 

For historical data, I used the SPDR S&P 500 Trust ETF (a.k.a. SPY) between Jan 2010 - Sep 2021. This is one of the most popular funds which aims to track the S&P 500 Index.


### Simple Moving Average (SMA)

The most basic moving average is the Simple Moving Average (SMA). The $N$-day SMA of close prices on day $t$ is given by,

$$
SMA_{t, N} = \frac{1}{N} \sum_{i = 0}^{N-1} C_{t-i} 
$$

Let's see how a 50 and 200 SMA crossover strategy does. 

![](/Images/MovingAverages/SMA.png?raw=true)

A potential drawback of SMA is that it assigns equal weighting to all of the days. Why should the oldest data and more recent data be treated equally? We might want to assigns less weight as we move further back in time. 

### Weighted Moving Average (WMA)

The WMA is an attempt to assign varying weights to data. In this method, the weight of each data point decreases linearly with each day. It is defined, 

$$
WMA_{t, N} = \frac{\sum_{i = 0}^{N-1}w_{i} C_{t-i}}{\sum_{i = 0}^{N-1} w_i}
\qquad \quad
\text{where }
\quad
w_i = N-i
$$

Let's see how a 50 and 200 day WMA crossover strategy does. 

![](/Images/MovingAverages/WMA.png)

### Exponential Moving Average (EMA)
The EMA is another way to assign greater weightings to more recent data. It is given by, 

$$
EMA_{t, \alpha} = \alpha \times C_{t} - (1 - \alpha) \times EMA_{t-1, \alpha}
$$

where $\alpha$ is some constant decay factor. In this method, the weight of each data point exponentially decreases with each day. 

Let's see how a 50 and 200 day EMA crossover strategy does. 

![](/Images/MovingAverages/EMA.png)

### Double Exponential Moving Average (DEMA) 

All of the moving average techniques mentioned above have some non-zero positve lag. A moving average without lag requires using future data points. However, it is possible to approximate a non-lagging moving average. The DEMA uses both single and double EMA components. 

$$
DEMA = 2 \times EMA1 - EMA2
$$

where 
$$ 
\begin{align*}
  &EMA1 = N \text{-day EMA of close prices} \\
  &EMA2 = N \text{-day EMA of EMA1}
\end{align*}
$$

This is the most elaborate method. Let's see how a 50 and 200 day DEMA crossover strategy does. 

![](/Images/MovingAverages/DEMA.png)


### Returns 

We've seen the generated signals of the moving average methods that have been mentioned. The best way to compare their performance is by considering their cumulative log and relative returns.

![](/Images/MovingAverages/Returns.png)

Here's a table of each strategy's total return and their average return per year: 

| Strategy      | Total Return (%)   | Average Yearly Return (%) |
| --------------| -------------------|---------------------------|
| SMA           | 47.89              | 4.24                      |
| WMA           | 41.84              | 3.71                      |
| EMA           | 55.59              | 4.92                      |
| DEMA          | -1.82              | -0.16                     |


From the data, we can see that although DEMA was the most elaborate, it's the worst strategy by a significant margin. SMA and WMA are both relatively similar, but EMA is definitely the best strategy to employ. 

I didn't factor in transaction costs into my returns calculations but I suspect that if I had, none of these strategies would have been particularly profitable!

Lastly, all of these strategies are really simple. It's probable that using different lengths of moving averages (as opposed to 50 and 200) would yield better results. Also, mixing and matching different moving averages could also be of use. 