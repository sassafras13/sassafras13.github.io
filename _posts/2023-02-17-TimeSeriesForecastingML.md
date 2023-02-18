---
title: Using ML Models for Time Series Forecasting 
layout: post
---

I have recently been doing a deep dive into time series forecasting with machine learning. I have discussed some of the basic principles, some considerations for pre-processing data, and techniques for performing an EDA on time series data. Today, I want to write about how to apply familiar ML models to time series forecasting. On the surface I assumed that this would be a simple process, but as we’ll see in a minute there is a paradigm shift that has to be made to think about predicting the future, and a lot of nuance to applying ML to this particular problem. Let’s get started!

## Time Series Forecasting is Supervised Learning

Brownlee reveals to us that time series forecasting can be posed as a supervised learning problem to ML models [1]. The challenge, as we will see soon, is how to prepare our data to fit into the supervised learning paradigm so that we can apply our usual ML tools to it. As you recall, the supervised learning framework involves giving a model some input $$X$$, and asking it to learn to predict a target variable, $$y$$ [1]. 

## The Sliding Window Method

So how do we apply this framework to a time series? One approach is known as the **sliding window method** or the **lag method** [1]. As an example, let’s consider that we have the daily average temperature in our neighborhood as a time series for the last 10 days: 

$$X = [40, 35, 37, 28, 24, 30, 32, 45, 37, 38]$$

How do we convert this data to both input and output? We can define the target variable to be the next entry in the series, like so [1]: 

$$X = [?, 40, 35, 37, 28, 24, 30, 32, 45, 37, 38]$$  

$$y = [40, 35, 37, 28, 24, 30, 32, 45, 37, 38, ?]$$

Now, for every entry in $$X$$ (excluding the first and last) the corresponding entry in $$y$$ is the next day’s temperature [1]. The order of the time series is preserved in both $$X$$ and $$y$$ [1]. We will want to delete the first and last entries in both time series since we are missing entries there [1]. The lag in this method refers to the number of previous time steps there are - in this case, we have a lag of 1 [1]. 

The Pandas library has a useful function for doing this automatically, known as the ```shift()``` function, which can make copies of columns and pull the rows forwards or backwards in time [2]. 

## The Sliding Window Method for Multivariate Time Series

Now that we have a basic grasp of the sliding window method, let’s apply it to multivariate time series - that is, time series with multiple features. Brownlee argues that ML is particularly well suited, compared to classical methods, for modeling multivariate time series data [1]. We can use the same sliding window approach for multivariate time series, and we can do so to predict either one or multiple features [1]. As an example, let’s imagine we now have the daily average relative humidity data in addition to our temperature data: 

$$X = \begin{bmatrix}  40 & 35 & 37 & 28 & 24 & 30 & 32 & 45 & 37 & 38 \cr 0.4 & 0.32 & 0.38 & 0.2 & 0.1 & 0.15 & 0.4 & 0.39 & 0.38 & 0.28 \end{bmatrix}$$

If our goal was to predict only the temperature data, then we could apply the sliding window and make the lagged temperature data our target variable, $$y$$ [1]. But if we wanted to predict both the input variables, then we could just set both the lagged versions of the inputs as the target variables [1]. 

So far this approach has allowed us to predict one step into the future, but what if we want to predict multiple steps into the future? 

## Multi-Step Forecasting

Continuing with the sliding window method, one way we could increase the range of our forecasts - for example from one day to two days in the future - would be to create two target variables, with one and then two days’ lag, as follows [1]: 

$$X = [?, 40, 35, 37, 28, 24, 30, 32, 45, 37, 38]$$  

$$y_1 = [40, 35, 37, 28, 24, 30, 32, 45, 37, 38, ?]$$

$$y_2 = [35, 37, 28, 24, 30, 32, 45, 37, 38, ?, ?]$$

Brownlee points out that while this technique can work, we’re now putting a higher “burden” on the input data to help us predict more output data [1]. Zulkifli recommends having a lag that is long enough to capture the period of a cycle of the data, whatever that might be [5]. 

## Data Pre-Processing Considerations for Sliding Window Methods

One thing we wanted to mention is that when we prepare data for time series forecasting with ML models, there are a couple best practices to use. First, the data should be scaled or normalized on input [5]. Next, we should try to provide stationary data to the model, so we should detrend the input data if possible [5]. (I’m not sure if these trends should be added again after the predictions have been made?)

## Using Random Forests to Generate Time Series Forecasts

Now that we know how to prepare a dataset for time series forecasting, let’s understand what it means to use a common ML model - random forests - to make a time series prediction. The random forest is a popular model in industry because it is highly interpretable, since you can interrogate the tree structure and understand where the tree is splitting the dataset [5]. However, as Mwiti points out, random forests are not capable of extrapolation, because they are designed to make predictions by taking inputs and using them to output a leaf node whose value is based on the values of the input data [6]. This means that, while we can use random forests to make predictions based on the training data that we have, we should be prepared, in practice, to retrain these models often as new data comes in [6]. 

Recall that random forests are an ensemble model that use decision trees as the individual learners [3]. Each decision tree in the ensemble is trained on bootstrapped data - that is, data sampled from the dataset with replacement - and then the predictions made by each individual decision tree are combined together [3]. We often allow the decision trees inside the random forest to overfit somewhat on the bootstrapped data, which, in practice, means that we do not prune the trees [3]. In a random forest, the bootstrapping method will also choose a subset of features, so that not all decision trees are given the same features during training [3]. 

When training the random forest, we have to be careful to segment the data while preserving the ordering for validation [3]. The typical cross-validation method, k-fold cross-validation, would not be appropriate here because it randomizes the segmented data so the model might be presented with data from a later time point and asked to predict an earlier sequence [3]. There are two alternative approaches to performing evaluation with time series data. The first is to create multiple train-test splits, where each time the train data occurs before the test data [4]. The test data is the same size for each split, but the train data will increase in size as we split the data later and later in the series [4]. 

An alternative to having multiple train-test splits is to use the **feed-forward** or **walk-forward** validation method [3]. As before, we divide the data into train and test sets by splitting at a particular point in time, but this time the train data can be at least some minimum size or a fixed size, and each train-test split has a constant size and looks like a rolling window was applied to the time series [4]. The feed-forward method is more robust, but can also be more computationally expensive because the rolling window will only be shifted a limited number of steps forward each time, requiring many models to be built, trained and evaluated in the process [4]. 

Note that together, both these validation processes are known as **backtesting**, because we are checking the performance of the model on historical data [4]. 

## References

[1] Brownlee, J. “Time Series Forecasting as Supervised Learning.” Machine Learning Mastery, 15 Aug 2020. <https://machinelearningmastery.com/time-series-forecasting-supervised-learning/> Visited 17 Feb 2023. 

[2] Brownlee, J. “How to Convert a Time Series to a Supervised Learning Problem in Python.” Machine Learning Mastery, 21 Aug 2019. <https://machinelearningmastery.com/convert-time-series-supervised-learning-problem-python/> Visited 17 Feb 2023. 

[3] Brownlee, J. “Random Forest for Time Series Forecasting.” Machine Learning Mastery, 2 Nov 2020. <https://machinelearningmastery.com/random-forest-for-time-series-forecasting/> Visited 17 Feb 2023. 

[4] Brownlee, J. “How to Backtest Machine Learning Models for Time Series Forecasting.” Machine Learning Mastery, 28 Aug 2019. <https://machinelearningmastery.com/backtest-machine-learning-models-time-series-forecasting/> Visited 17 Feb 2023. 

[5] Zulkifli, H. “Multivariate Time Series Forecasting Using Random Forest.” Towards Data Science, 31 Mar 2019. <https://towardsdatascience.com/multivariate-time-series-forecasting-using-random-forest-2372f3ecbad1> Visited 17 Feb 2023. 

[6] Mwiti, D. “Random Forest Regression: When Does It Fail and Why?” Neptune.ai, 31 Jan 2023. <https://neptune.ai/blog/random-forest-regression-when-does-it-fail-and-why> Visited 17 Feb 2023. 
