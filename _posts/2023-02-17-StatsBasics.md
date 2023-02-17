---
title: A Review of the Fundamentals of Statistics
layout: post
---

I need to review some common statistical tests for comparing samples, but before I do that, I wanted to start with a brief overview of some key statistical concepts. I will outline where statistics fit into the scientific method, the fundamental ideas behind a statistical test, parametric and non-parametric tests, as well as different types of data that we can analyze with statistics. 

## The Scientific Method, with Statistics 

The general steps in the scientific method are [3]: 

1. Define the research question.   
2. Formulate the hypothesis.   
3. Design the experiment.   
4. Collect data.   
5. Analyze the data.   
6. Make conclusions.   

In this process, we use statistics to formulate the hypothesis (step 2) and to choose the sample size as part of the experimental design (step 3), as well as to analyze the results (step 5) [3]. In step 2, we want to clearly define the hypothesis in a way that we can accept or reject it using the results of our experiment, and we may need to define several, connected hypotheses to address the research question [3]. We will discuss this process in more detail in the next section.

In step 3, we need to collect sufficient samples (observations, data) to be able to rigorously accept or reject the hypothesis [3]. To do this, the samples must be [3]:

1. Random   
2. Representative   
3. Sufficiently large   

The first item, randomness, is necessary because probability and statistics are based on the idea that the samples are drawn randomly and that any sample that we draw is equally likely to be drawn as any other sample [3]. Of course, sometimes it is impossible to be drawing purely random samples and so in this case you should proceed cautiously and report the fact that you know the samples are not purely random as part of your results [3]. The second item, that the samples are representative of the entire population they are drawn from, is also likewise somewhat impossible in practice, so we instead accept that likely if we get enough data, it will be reasonably representative of the population [3]. This leads us to our third item, the size of the data we collect. Here, we do not want to just collect a massive dataset - the point of statistics is to intelligently size the sample - and so we will want to determine the size of data we should collected based on factors such as the size of the difference we are trying to observe, the variation in our target variable, the type of statistical analysis we are doing, and the sampling cost [3]. 

And step 5, analyzing the data, is where the bulk of statistics operates. 

## The Basics of a Statistical Test

In statistics, we will often derive a **null hypothesis** from the research hypotheses [3]. The null hypothesis assumes that there is no difference between two groups, and that any differences that are observed are purely by chance [3]. For example, if we are testing whether a vaccine is effective against a disease by inoculating a control group with saline and a test group with the vaccine, the null hypothesis is that these two groups will not show any meaningful difference in their susceptibility to the disease [3]. We often refer to the null hypothesis as “H-nought” or $$H_0$$ [3]. To be specific, in our example, $$H_0$$ states that [3]: 

$$H_0 : \pi_1 = \pi_C$$

Where $$\pi_1$$ is the number of patients with the disease in the treated group, and $$\pi_C$$ is the number of patients with the disease in the control group [3]. Given data about the number of patients in each group and the incidence rate of disease in each group, we can compute the probability that the null hypothesis is true [3]. For example, let’s say that for 10,000 patients in each group, we see that $$\pi_1 = 0.01$$ and $$\pi_C = 0.04$$ - that is, the treated group had a quarter of the incidence rate of the control group. The probability that this outcome was purely by coincidence (i.e. the probability of the null hypothesis) is very small, and so we can **reject** the null hypothesis and confirm that there is a statistically significant reason why the test group was healthier than the control group [3]. 

More specifically, we typically need to establish _a priori_ a probability threshold, below which we reject the null hypothesis (i.e. the probability that the two groups were different is entirely by chance), and above which we accept the null hypothesis. Put another way, if the probability that the difference between the two groups is entirely due to chance is very small, then we can reject this null hypothesis, but if the probability is high that the measured difference is pure coincidence, then we accept the null hypothesis [3]. If we can reject the null hypothesis, then we can usually accept our experimental hypothesis instead (i.e. that this vaccine is an effective treatment for the disease) [3]. 

It is worth noting that this statistical test of either accepting or rejecting the null hypothesis is really asking the question of whether we would see similar results that differ so much from the null hypothesis again if we repeated the experiment [3]. It is also important to remember that accepting or rejecting a hypothesis is not the same as _proving_ a hypothesis [3]. Since this method involves probability, there is always a non-zero possibility that the hypothesis we rejected was actually true, and vice versa [3]. 

There are two ways that researchers will typically assess the null hypothesis. The first is to compute the probability that the results were measured if the null hypothesis is true, and then use this probability to accept or reject the null hypothesis [3]. This probability is often called either the **significance level** or the **P value** [3]. When the P value is large, then we accept the null hypothesis [3]. Conversely, a more “decisive” approach (according to Dowdy and Wearden) is to set a **rejection level** before the experiment is conducted [3]. Then given the experimental data, a probability is computed and if it is below this predetermined level, the null hypothesis is rejected [3]. This method was more in favor when computers were not yet widely used for computation. 

Depending on your research project, you may have specific hypotheses that you want to verify, or you may simply want to explore the data that you have [2]. If you have specific hypotheses, then you can use statistical tests to accept or reject them, but you should be careful of trying to test too many hypotheses with the same dataset [2]. For example, Campbell and Shantikumar recommend using a Bonferroni correction, which recommends setting a significance level for $$n$$ independent hypotheses as $$0.05 / n$$ [2]. So, for example, when testing two independent hypotheses, in each case the result is significant only if $$P < 0.025$$ [2]. 

## Parametric and Non-Parametric Statistical Tests

There are a range of statistical tests available for comparing sets of data [1]. They often use different measures of the data, such as the mean or standard deviation, to compare against some criteria to determine if the two datasets are similar or not [1]. Broadly speaking, there are two classes of statistical tests: **parametric** and **non-parametric** tests [1]. The term “parametric” means that the test (or model, in the case of machine learning) makes some specific assumptions about the data being processed which affect the way the test is performed [2]. For example, many parametric tests (as we will see shortly) assume that the data is normally distributed [2]. Conversely, non-parametric tests make no such assumptions, and so might be a better choice if you don’t know how your data is distributed (or you know it is not normally distributed) [2]. 

## Types of Data

Once the hypothesis has been defined, we need to consider what kinds of data we are going to collect (following steps 3-4 in the scientific method above) [2]. There are different taxonomies for classifying data that we can collect through measurement. One posed by Stevens has 4 types of data (nominal, ordinal, interval and ratio) [6] - others might be nominal, ordinal, quantitative (discrete) and quantitative (continuous) [2,3]. I was not sure what nominal and ordinal meant so I wanted to define them here: 

* **Nominal:** Nominal data consists of labels or names applied to data points that are not numeric [4]. Typically the labels come from different categories that don’t overlap, and that do not have any hierarchical ranking [4]. We could only measure the central tendency of nominal data using the mode [4].    

* **Ordinal:** Similar to nominal data, ordinal data is also a set of labels used to qualitatively describe data points [5]. However, unlike nominal data, there is an assumed ranking to the ordinal scale [5]. 

The type of data we are analyzing will affect the type of comparison test that we choose. Let’s do a brief survey of the types of comparison tests available to us. 

## References

[1] Sirisilla, S. “7 Ways to Choose the Right Statistical Test for Your Research Study.” Enago.com. 4 Jan 2023. <https://www.enago.com/academy/right-statistical-test/> Visited 17 Feb 2023. 

[2] Campbell, M.J., Shantikumar, S. “Parametric and Non-parametric tests for comparing two or more groups.” HealthKnowledge, 2016. <https://www.healthknowledge.org.uk/public-health-textbook/research-methods/1b-statistical-methods/parametric-nonparametric-tests> Visited 17 Feb 2023. 

[3] Dowdy, S., Wearden, S. Statistics for Research, Second Edition. John Wiley & Sons, 1991. 

[4] “What is Nominal Data? Definition, Examples, Variables & Analysis.” Siplilearn, 30 Sept 2022. <https://www.simplilearn.com/what-is-nominal-data-article> Visited 17 Feb 2023. 

[5] “Ordinal data.” Wikipedia. <https://en.wikipedia.org/wiki/Ordinal_data> Visited 17 Feb 2023. 

[6] “Statistical data type.” Wikipedia. <https://en.wikipedia.org/wiki/Statistical_data_type> Visited 17 Feb 2023. 
