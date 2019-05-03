---
title: "Algorithmic Trading Using Quantitaive Analysis"
date: 2019-05-01
tags: [machine learning, finance, neural networks]
header:
  image: "/images/quant-analysis/header.png"
excerpt: "Machine Learning, Algorithmic Trading"
---
## Overview

With the rise in machine power, algorithmic trading has become a staple in the finance sector as a way to carry out large sequences of trades based on pre-written algorithmic rules.
With the use of technical indicators commonly used in quantitative analysis, I created a neural network model that predicts stock price trend and creates buy and sell signals
based on said trend.

### Clustering stocks based on daily returns and volatility

First, I clustered companies from the S&P 500 based around their daily returns and volatility using K-means. While trying the minimize inertia, the within-clusters sum of squares, I get
6 clusters being the most optimal. The graph below shows how each company is separated and the model seems to do this mainly based around returns.
![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/cluster-results.png)
