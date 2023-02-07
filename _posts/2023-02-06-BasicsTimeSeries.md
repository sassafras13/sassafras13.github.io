---
title: A Brief Introduction to Time Series Forecasting
layout: post
---

In this post I want to introduce some of the concepts underpinning time series forecasting. I predict that this will be the first in a series of posts on this topic. In this post we will discuss some key concepts, discuss importing a dataset and imputing missing values. 

Time series forecasting assumes that there is a **data generating process** (GDP) which is the underlying process that is producing the data we are analyzing. We do not know the actual DGP completely, and instead we are going to build models to try to capture it as accurately as possible [1,2].

One way to model time series data is using an autoregressive signal: a signal that depends on values at previous time steps, specifically linearly dependent on values at previous time steps, plus some stochastic term [1,3]. (Different from a moving average because it may contain a unit root – we’ll discuss this in more detail in a future post [3].) 

Most modeling approaches assume that the DGP is stationary, but this can often be broken in practice, either when the mean or the variance (heteroscedasticity) of the distribution changes over time [1]. 

## Process for Importing a Dataset

We talk here about importing a dataset to your work environment, cleaning missing values and other initial steps we take before performing an exploratory data analysis. 

1. Understand the features in the dataset. Read the documentation, understand the relationships between features in your dataset.   

2.  Check the data types of the features in the dataset. Convert dates to a datetime format if they are not already in that form. This allows you to obtain information related to the date/time using packages like Pandas [1] - not just day/month/year either, but day of the week, etc. 

3. Use Pandas’ ```describe()``` and ```info()``` functions to see the basic statistics and a summary of every feature in your dataset. These will help you get a preliminary understanding of the dataset, and can also help you detect any missing values.    

4. Find and fill in missing values. See more detailed discussion below.

## Handling Missing Values

The first thing to ask is whether the values are really missing or not [1]. It might be that there is no data for a particular time period because there really was no data [1]. For example, if you are looking at data for books checked out of the library, not all books will be checked out all the time, so if there is no data for a particular book for a month, it might simply mean that no one was borrowing the book during that time. 

Another thing to consider is whether or not there is a pattern in how and when the data is missing [1]. For example, if no books are checked out on Sunday because the library is closed that day, if we fill in the data for that day with a 0, then our time series forecasting model, which may rely on patterns from previous days, may predict that we will not see any books checked out on Monday either [1]. One way to address this might simply be to ensure our modeling approach knows the days of the week that the data corresponds to so it can learn this trend as well - and so filling in with zeros will be acceptable in this case [1].

Now if we assume that we have found missing values that we really do need to address, there are a number of ways to handle them [1]:

* **Forward fill**: This method uses the last value that we have in the time series and uses it to fill all the missing values until we find the next value in our dataset.    

* **Backward fill**: This is the same as above except we’re taking the next value after the missing data and using it to fill in the missing values.   

* **Mean value fill**: Here we take the mean of the **entire** time series and use it to fill in the missing values.    

* **Linear interpolation**: We fit a line to the values on either side of the missing data and use the line to interpolate the values of the missing points.   

* **Nearest interpolation**:  This method is a combination of forward and backward fill, where we choose the closest value that isn’t missing and use it to fill in the missing value. 

There are also a range of other interpolation techniques available in most data science libraries including fitting **splines** or **polynomials** to the entire dataset and using those fits to impute the missing data [1]. 

## References
[1] Joseph, M. Modern Time Series Forecasting with Python : Explore Industry-Ready Time Series Forecasting Using Modern Machine Learning and Deep Learning. 1st ed. Birmingham: Packt Publishing, Limited, 2022.

[2] “Data generating process.” Wikipedia. <https://en.wikipedia.org/wiki/Data_generating_process> Visited 30 Jan 2023.  

[3] “Autoregressive model.” Wikipedia. <https://en.wikipedia.org/wiki/Autoregressive_model> Visited 30 Jan 2023. 
