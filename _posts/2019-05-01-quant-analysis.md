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

## Exploratory Data Analysis

First, I scrape all the companies using beautiful soup and then load in the data from [Quandl](https://www.quandl.com/). Then I try to cluster the data around their yearly returns and volatility to see if the stocks can be stratified in a meaningful manner.
Using K-means and while trying the minimize inertia, the within-clusters sum of squares, I get
6 clusters being the most optimal. The graph below shows how each company is separated and the model seems to do this mainly based around returns.

![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/cluster-results.PNG)

### Trading Strategies

There are 4 main types of technical indicators I use to test these strategies

1. Trend overlays: Moving averages (MA) is the primary method to find price trends.
2. Momentum: Relative Strength Index (RSI) is used to capture the  price momentum of stocks
3. Volume: The total volume of stock can be a great indicator for price movement
4. Volatility: The volatility, or the price range, is another good measure to analyze trends. In this case, Average True Range(ATR) is used

## Modeling

Stock data is usually very noisy so one way to model this information is through ARIMA models. These models take into account past values and error in relation to those values in order to predict how a sequence of data will behave in the future. Stock data doesn't usually experience seasonality, so we will use the regular ARIMA method to see how our results pan out. For the rest of the analysis, Apple's stock data was used to analyze the strategies and implement the models.

After searching through the best possible combination of values for the AR, I, and MA models, we land at ARIMA(4,1,2). The results, however, aren't all that great.
![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/arima.PNG)

What the model seems to be doing is finding the best fit line for the previous data and using that trend as the prediction for the future. ARIMA does not seem to be the best model to use, so I try a newer method. LSTM neural networks are a type of recurrent neural network that use memory when dealing with sequences in order to learn how the price shifts.  
We tune the model using the following parameters:
![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/lstm-params.PNG)
Using a time-step of 60 trading days for each data point, the model is used to predict 60 days of stock close price for Apple.

![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/lstm-results.PNG)

Clearly this model is much better than ARIMA in terms of predicting price trends. The sequence is data is accurately predicted due to a sort of memory of past events used to judge how the future might be affected.
The final step is to back-test the strategies mentioned before to see how well they perform. For backtesting, the [Quantopian](https://www.quantopian.com/) platform was used to conduct trades using our algorithmic rules. The strategies are tested from January 2017 to March 2019 with a base capital investment of $5000. The reason a small base capital is used is due to the survivorship bias. This bias focuses too much on past data as a great indicator for the future, but does not take into account the actions taken be users of the model in making financial moves. If an investment firm makes large trades based on the models, then the results of the future are changed and if many financial entities conduct their actions based on these models, then our models become essentially useless and need to be constantly refed the data in order to be more accurate again.

### Kaufman Adaptive Moving Average (KAMA)

![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/kama_returns.png)

The KAMA strategy seems to have a decent gain of 60% over a 2 year period.

### RSI

![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/rsi_returns.png)

The RSI strategy seems to be a bit more of a conservative strategy and results in about 40% profit over the 2 year period.

### Volatility (ATR)

![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/volatility_returns.png)

The volatility based strategy seems to perform better with returns of almost 58% and having the benefit of relying on volatile swings to make trades.

### Volume

![alt]({{ site.url }}{{ site.baseurl }}/images/quant-analysis/volume_results.png)

The volume based strategy seems to be the best with returns of almost 85%.

## Conclusion

Using multiple strategies from technical analysis, we can conduct algorithmic trades based on a variety of indicators. We can also use LSTM models to gauge how the price may change in the future in order to reassess our strategies. We also see that traditional time series analysis methods such as ARIMA don't work quite well with data as noisy as stock data. For more in depth analysis of the strategies as well as tuning of the LSTM, check out the full notebook below.
[GitHub](https://github.com/sohaibk321/quant_analysis_stocks)
