---
title: A Brief Survey of Machine Learning Regression Models
layout: post
---

I want to use this post to survey some common machine learning regression models to better understand the range of tools available and when we would choose certain models. Regression refers to finding a function that can predict a continuous output for a given input, for example predicting the price of a house given knowledge of its square footage and number of bedrooms [1]. There are a range of machine learning models that can be adapted to perform either classification or regression, and they are suitable for different settings. I’m going to explore some categories of models here, with a focus on models that are well suited to small datasets. Let’s start exploring!

## Linear Models
We’ll start by considering the classic **linear regression** model because this will help ground us in thinking about regression and it will serve as a good point of comparison with more advanced models in a minute. In general, a regression algorithm is trying to learn a function that maps $$X \rightarrow y$$, i.e. $$y = f(X)$$. In linear regression, that function is simply a line, written as $$y = w_{1:n}X + w_0$$ (we assume that $$X$$ may be a matrix of data points with multiple features, and that $$w_{0:n}$$ is a vector of coefficients). To find the coefficients, we need to solve the following minimization problem [2]:

$$\min_{w} \|Xw - y\|_2^2$$

We can make this model more robust by adding different forms of [regularization](https://sassafras13.github.io/Regularization/), including Ridge regression (the L2 norm) and LASSO regression (the L1 norm). The Ridge regression term (that we would add to the minimization expression above) can be written as $$\alpha \|w\|_2^2$$ where larger values of $$\alpha$$ will increase the regularization penalty and force the coefficients to be smaller. With Ridge regression, no term is ever driven completely to zero, in contrast with LASSO, which can be used to create more sparse linear models that eliminate some features completely. This is possible because LASSO regression (Least Absolute Shrinkage and Selection Operator) uses the L1 norm which can drive coefficients to zero, i.e. $$\alpha \|w\|_1$$ [2]. 

## Support Vector Machines

Support vector machines (SVMs) look for a hyperplane that separates data points while maximizing the gap (or margin) between any data point and the hyperplane [3]. For example, given a dataset $$X$$ and a set of binary labels, $$y$$, we want to find a hyperplane that separates the points $$X$$ labeled $$y = 1$$ from those labeled $$y=0$$ by the maximum available margin. We can enforce this separation as a **hard margin**, where no points in $$X$$ may fall on the wrong side of the hyperplane, or as a **soft margin**, where points are allowed to lie on the wrong side of the hyperplane, subject to some penalty [3]. 

In a linear example, the hyperplane would be defined by [3]:

$$w^T X - b = 0$$ 

Where $$w$$ is the normal vector to the hyperplane and $$b$$ is the offset. The normal vector learned by the SVM can be used to make predictions, thereby performing regularization as follows [3]:

$$\min \frac{1}{2} \|w\|^2$$
$$\text{subject to } |y_i - \langle w, x_i \rangle - b| \leq \epsilon$$

Where $$\epsilon$$ is a hyperparameter that defines the margin of error - all predictions, $$\langle w, x_i \rangle + b$$ must be within $$\epsilon$$ of the true value $$y_i$$ [3]. 

Note that SVMs can also generalize well to nonlinear situations through use of the **kernel trick**. This technique uses some kernel to transform the dot product $$w^T X$$ with a nonlinear function that transforms the feature space to some higher-dimensional representation. In this high-dimensional space, it may be easier to find a hyperplane that separates the two categories than in lower-dimensional space [4].

Some advantages to using SVMs are [5]:

* Work well for high-dimensional data, even when the number of dimensions exceeds the number of data points available.    
* Only a subset of the data points are used to find the hyperplane (i.e., only those closest to the hyperplane) so this is a memory efficient technique.   
* The fact that we can swap in different kernels makes this a versatile model.   

And some disadvantages [5]:

* If the number of features is significantly larger than the size of the dataset, be careful of overfitting. This affects the choice of kernel and regularization term.   
* SVMs do not compute probabilities directly, if that’s something you need from your model.   

## Nearest Neighbors

The basic idea behind the nearest neighbors algorithm is to find some predetermined number of training samples (i.e. from $$X$$) that are “close” to the query point, and predict the label for the new point from these neighbors. There are multiple choices for the number of neighbors, and the metric for how “close” they are to the query point, yielding many variants on this approach. Commonly, we define a constant number of neighbors, $$k$$, which gives us “k-nearest neighbor learning”, and we can use measures like the Euclidean distance (L2 norm) to compute how close two points are to each other. Since this algorithm applies labels simply by comparing new points to existing points, but it does not fit an analytical expression with parameters, we call this a **non-parametric** model. In particular, this model is great for situations where the decision boundary (the line that divides classes) is very irregular [6]. 

When we consider the regression form of nearest neighbors, it is essentially the same as described above, with the specific clarification that the predicted “label” for the new query point is simply $$y$$, some continuous value that represents the output of the function $$f(X)$$. In some cases, it may be helpful to add weights to the algorithm such that certain neighboring points are given more weight than others, for example if they are closer to the query point [6]. 

## Gaussian Processes

We recently encountered Gaussian Processes (GPs) in our discussion of [Bayesian optimization](https://sassafras13.github.io/BayesianOptimization/) where I provide a brief overview of the technique. I will also try to write a post dedicated to them soon because they are cool applications of the normal distribution. For the purposes of this post, let’s just say that GPs are a collection of normal distributions that each correspond to a single data point, and the covariance matrix for all of these distributions conveys information about the probability that each function is the true function. The covariance can be computed using various kernel methods. 

GPs can be used for regression by fitting a GP model to a training dataset, $$X$$. Specifically, the parameters of the covariance function (i.e. the kernel) are optimized to fit the data. We can then make predictions from the model by drawing samples from the joint probability distribution represented by the fitted GP [7]. 

The advantages to using GPs are [7]:

* Since we are using a continuous probability distribution, we can easily interpolate between data points.   
* The predictions generated by a GP are probabilities so we can compute confidence intervals to accompany our predictions.   
* We can use different kernels to define the covariance of the GP which gives us versatility in this type of model.   

Some disadvantages include [7]: 

* This kind of model is not sparse, it uses every data point available and every feature to make a prediction.   
* These models do not work well in high-dimensional spaces.   

## Decision Trees

Decision trees are a popular form of ML model because they are very easy to interpret. This is because they use a series of yes/no decisions in a tree structure to take features of a data point and use them to predict the target value (i.e. they use features of $$X$$ to predict $$y$$). Decision trees can be used to perform either classification or regression - in the case of regression, the leaves of the tree will take on continuous values corresponding to $$y$$ [8]. 

The algorithm for building decision trees uses some measure of the benefit of splitting the data into categories by applying a threshold to individual features. For example, if we want to predict whether or not a sporting event will occur based on the weather (outlook, temperature, humidity and wind), we can examine how splitting the data based on whether the temperature was hot or cold will generate sub-groups of the data, and how much information we gain by doing so. Different forms of the decision tree model will use different metrics, for example, Gini impurity or information entropy, to compute which split (based on outlook, temperature, humidity or wind) will yield the greatest gain in information at each step in the tree [8]. 

Some of the advantages of decision trees include [8,9]:

* They are very easy to interpret because we can visualize all of the decisions that the model learns to make.   
* Minimal data preparation is needed to apply this model, although it will not work well if there are missing values in the dataset.   
* Works well with large datasets.   
* Decision trees can work with both numeric and categorical data natively.   

Some disadvantages include [8,9]:

* They can overfit so techniques like pruning or limiting the depth of the tree may be necessary to regularize the model.   
* A single decision tree can be unstable because small variations in the values in the dataset could lead to a completely different tree being generated. This can be addressed using ensemble methods as we will see in the next section.   
* No model is great at extrapolation but decision trees are particularly bad because they’re giving piecewise estimates of the function $$y = f(X)$$.   
* There is no guarantee that the decision tree found during training will be globally optimal - another good reason to use an ensemble method.   
* Decision trees do not work well with unbalanced dataset (i.e. where most of the points fall into one category for a particular feature).   

## Ensemble Methods (Random Forest)

As we saw in the previous section, a single decision tree can be an unstable, unreliable model for a given dataset. It can help, in this case, to use an ensemble method that joins the predictions of several variations of a model (or models) together and returns some final prediction based on all the individual models’ predictions. I don’t want to cover all the approaches to ensemble learning here, instead I will focus on how we commonly build ensemble models out of decision trees - this ensemble version is called a random forest. 

In a random forest, we build multiple decision trees, and we use a couple of techniques to introduce variability in how the trees are formed. We then take an average over all the predictions from all of the trees and use that averaged prediction as the final value for the entire random forest. The variability is introduced in two places: first by drawing a subset of training samples from the full dataset $$X$$ (with replacement - this is called **bootstrapping**) and using this subset to train the tree. Second, we can choose to use a random selection of features in the dataset to train a particular tree, thereby generating some variation in how the tree can split branches [10]. 

## Conclusion
Thank you for reading along with me today. I realize that I did not dig into any one model in great detail, instead my goal was to remind myself of how each regression model worked and where they might be most useful. Thanks for reading. 

## References

[1] Brownlee, J. “Difference Between Classification and Regression in Machine Learning.” 11 Dec 2017. <https://machinelearningmastery.com/classification-versus-regression-in-machine-learning/> Visited 4 Jun 2022. 

[2] “Linear Models.” Scikit-Learn 1.1.1. <https://scikit-learn.org/stable/modules/linear_model.html> Visited 4 Jun 2022. 

[3] “Support-vector machine.” Wikipedia. <https://en.wikipedia.org/wiki/Support-vector_machine> Visited 4 Jun 2022. 

[4] “Kernel method.” Wikipedia. <https://en.wikipedia.org/wiki/Kernel_method#Mathematics:_the_kernel_trick> Visited 4 Jun 2022. 

[5] “Support Vector Machines.” Scikit-Learn 1.1.1. <https://scikit-learn.org/stable/modules/svm.html> Visited 4 Jun 2022. 

[6] “Nearest Neighbors.” Scikit-Learn 1.1.1. <https://scikit-learn.org/stable/modules/neighbors.html> Visited 4 Jun 2022. 

[7] “Gaussian Processes.” Scikit-Learn 1.1.1. <https://scikit-learn.org/stable/modules/gaussian_process.html> Visited 4 Jun 2022. 

[8] “Decision tree learning.” Wikipedia. <https://en.wikipedia.org/wiki/Decision_tree_learning> Visited 4 Jun 2022. 

[9] “Decision Trees.” Scikit-Learn 1.1.1. <https://scikit-learn.org/stable/modules/tree.html> Visited 4 Jun 2022. 

[10] “Ensemble methods.” Scikit-Learn 1.1.1. <https://scikit-learn.org/stable/modules/ensemble.html#forests-of-randomized-trees> Visited 4 Jun 2022. 
