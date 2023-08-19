# homework-algo-trade

## Summary

This is a homework attempting machine learning analysing market history to come up with algorithm trading strategy. 

In the exercise, we focus on the closing prices historical data of a provided dataset, which has 4223 data points of multiple timing prices through out 1033 days from 2015 to 2021.

The general method is to look at the percentage change rates, suppose a "signal of buy" (1.0) when the price gone up (from last to now point-in-time), "signal to sell" (-1.0) when the price gone down (from last to now point-in-time).

## Algorithms

We tried three ways: simple logic, machine learning using the Support Vector Machine, machine learning using the Gradient Boosting Classification. 

In machine learning, we apply the simple moving average (SMA) of the closing prices as two features to train the model, one short window one long window. The original "signal of buy/sell" is used as the labels. 

During the Support Vector Machine exploring, we tested two kinds of settings for the SMA, respecitively with the short window as 4, the long window as 100, and 50. We also tried different slicing of the data to train and test, and finally decided on SMA 4/50, train data points as 5 months. On the same setting, we then tried the Gradient Boosting Classification model.

Given the total 1033 days, usually the machine learning would make use of 75% for training and 25% for testing. However we have found that if we do too much training, the model could not predict even one -1.0, making the whole thing invalid. The speculated reason might be the wild uneven flucuation in data?

![Actual Return Raw Plot](Images/raw_plot.png)

The same happens using the LogisticRegression model, in which the model does not predict any sell signal. 

## Measurement

With a goal of making profit, we compare our strategy cummulative return with the actual in the historical data, in addition to a few common measurement of machine learning model such as the classification report. The idea is, if our algorithm catches all up and down accurately, the actual accumulative return would be the best case scenario. However, theoretically, better trading could beat the actual return using buy low sell high. 

In this exercise, the usual metrics of machine learning models are not very useful, since our labels are presumptuous in the first place. There is not much meaning comparing the set labels with the predicted labels.

## Study Report

### Simple Up Buy Down Sell

The simple buy/sell model appears not fit this particular prices curve. 

![simple](Images/no_ML.png)

### SVM SMA short/long window 4/100, train 3 months

The baseline situation looks okay. The strategic trading gains a bit more than the actual return.

![100_3m](Images/baseline.png)

### SVM SMA short/long window 4/100, train 10 months

When we increased the training data from 3 months to 10 months, the trategic trading performs worset than the 3 months, predicting based on the rest of the data. Something curious happens here, where the "actual returns" at the final point is higher than if we train for 3 months. A moment of double checking and contemplating reveals that, since our charts are based of cummulative returns, it actually has a dependency on the starting point (as the 1), namely how many ups and downs to make up the final point. In this light, if we stay put in the first 10 months, there is a higher chance to gain more profit despite any strategy.

![100_10m](Images/train_10_months.png)

### SVM SMA short/long window 4/50, train 5 months

When we reduced the long window of the simple moving average, from 100 to 50, we observe more fluctuation throughout the years, with our strategy vs actual, and a beat to the actual in the end. Personally I like this model, as it feels more real hence maybe works for the real unknown future.

![50_5m](Images/SMA_long50.png)

### Gradient Boosting

Overall, the new model appears not as good as the baseline SVM model, as it predicts less gain and even less than the actual return. It also appears worse than my tuned SMA4/50, train 5 months, SVM model. 

![gbc](Images/SMA50_train_5months.png)

### Raw Algo, SVM, Gradient Boosting

Here is a picture comparing raw algorithm, SVM, and Gradient Boosting Classification. 

![comp](Images/final_picture.png)
