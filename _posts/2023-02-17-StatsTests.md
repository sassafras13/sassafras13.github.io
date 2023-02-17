---
title: Common Statistical Hypothesis Tests
layout: post
---

In this post I want to survey some common statistical tests for comparing two or more samples from different populations to determine if they are different or not. This post builds on ideas I discussed in a [previous one](https://sassafras13.github.io/StatsBasics/). The comparison tests that we will review include: 

1. Student’s t-test   
2. ANOVA   
3. Z-test   
4. Chi-squared test

## Student’s t-test

The t-test is a common test for determining whether or not 2 groups are similar. The t-test compares the means of two groups [1]. The test was developed when William Sealy Gosset noted that if fewer than 30 random samples are drawn from a normally distributed population, then the distribution of the t statistic computed from these 30 random samples is _not_ normally distributed [2]. The t statistic is [2]: 

$$t = \frac{\bar{y} - \mu}{s / \sqrt{n}}$$

Gosset found that the tails of the distribution of $$t$$ for small $$n$$ were larger than for the standard normal distribution, but as $$n$$ increases the distribution becomes more normal [2]. The t distributions have several properties [2]: 

1. Unimodal   
2. Asymptotic to the horizontal   
3. Symmetrical about 0   
4. Dependent on the number of degrees of freedom, $$v$$, where in this case $$v = n - 1$$    
5. More variable than the standard normal distribution   
6. But approximately standard normal if the number of degrees of freedom is large    

We can use the t distribution to estimate probabilities for a non-normal distribution if it is, at least, symmetric, unimodal and does not have a very large variance [2]. There are tables available (or software) that list the critical t-values for different degrees of freedom and different probabilities that t exceeds the critical value (often referred to as $$\alpha$$) [2]. For example, the critical t-value for the case where there are 21 degrees of freedom and the probability that the measured t-value exceeds the critical value is 0.05 is $$t_{0.05, 21} = 1.721$$ [2]. Now let’s see how we can use it for comparison testing (it can be used for other things as well). 

### Matched (Paired) t-test
We can use the t-test to make inferences related to the mean of the difference between two _matched_ groups [2]. We can do this as long as our data is [2]: 

1. From a normal distribution (or at least a symmetric and unimodal one)   
2. From a population whose variance is unknown (we estimate it from our sample)   
3. Randomly sampled   

Matched groups will have some similarity between them, for example test scores for a set of students before and after taking a specific class [2]. The important thing is that both sets of data must have come from the same source, in some fashion, to remove variability that confounds the results [2]. Given the two groups of data, we can compute the difference between the two groups and the mean and variance of the difference. We can use this to compute the t-value and then compare it to the critical t-value for this particular number of degrees of freedom and our predetermined value of $$\alpha$$. If the calculated t-value exceeds the critical value, then we can reject the null hypothesis and say that there is truly a difference between the two groups [2]. Note that we can also use the t-test to compute **confidence intervals** as needed [2]. 

### Independent (Two Sample) t-test

There is another version of the t-test that allows us to compare samples from two different populations. In this case, we assume that the two populations are [2]: 

1. Independent    
2. (Approximately) normal   
3. Of unknown variance, but we assume the same variance in both populations    

In this case, the t-test can be written as [2]:

$$t = \frac{ (\bar{y}_1 - \bar{y}_2) - (\mu_1 - \mu_2) }  { \sqrt{ \frac{s_p^2}{n_1} + \frac{s_p^2}{n_2} } }$$

Where the variance, $$s_p$$, can be estimated as the **pooled sample variance** using a weighted average approach [2]: 

$$s_p^2 = \frac{(n_1 - 1)s_1^2 + (n_2 - 1)s_2^2}{n_1 + n_2 - 2}$$

We should note, however, that this test is not accurate if the variances of the two groups are different. This test should really only be used if we know the two samples are independent. 

## ANOVA

The Analysis of Variance (ANOVA) family of tests extends upon the capabilities of the t-test by allowing us to test hypotheses for 3 or more independent samples taken from different normal populations with different means but **common variances** [2]. In this scenario, the null hypothesis is that the means for all sets of samples are equal. 

Let’s understand why the ANOVA test focuses on variance, and what that means from a mathematical standpoint. Let’s assume we’re going to compare the time required to solve a maze for 3 groups of mice who have had increasing amounts of knowledge of the maze [2]. There is an average time to solve the maze computed over all the mice for all the groups ($$\bar{y}$$), and there is an average time to solve the maze for each set of mice ($$\bar{y}_i$$ for the $$i$$-th set of mice) [2].  Given this, we can write an additive model that describes the components contributing to the time to solve the maze for the $$j$$-th mouse in the $$i$$-th group [2]: 

$$y_{ij} = \bar{y} + (\bar{y}_i - \bar{y}) + \epsilon_{ij}$$

Here, the first term is the mean time to solve the maze for all the mice, the second term is the **mean treatment effect**, or the adjustment to the mean, for all the mice in the $$i$$-th group, and finally the third term captures the stochastic variation in each mouse’s performance [2]. Note that we can assign the variable $$\alpha_i$$ to the mean treatment effect term, and use it to rewrite the null hypothesis for this test [2]: 

$$H_0 : \alpha_1 = \alpha_2 = \alpha_3 = \alpha_4 = 0$$

If the mean treatment effect is the same for all the mice, and we assume that the stochastic noise, $$\epsilon_{ij}$$, is i.i.d. with mean 0 and variance $$\sigma^2$$, then this is equivalent to saying we assume the mean for each group is equal [2]. This formulation of the problem is called a **one-way completely randomized ANOVA** [2]. 

As we saw earlier, there are different averages that we can calculate for our dataset, and there is a corresponding variance for each of these averages. Specifically [2]: 

* **Total variance**: $$\frac{\sum_i \sum_j (y_{ij} - \bar{y})^2} {na - 1}$$ (where $$a$$ is the number of groups and $$n$$ is the number of samples within each group). This is the variance corresponding to the grand average for the entire dataset.      

* **Within-group variance**: $$\frac{ \sum_i \sum_j (y_{ij} - \bar{y})^2}{a (n - 1)}$$. This is the variance for the average over one group. It is also called the **pooled variance**.     

* **Among-group variance**: $$n \left[ \frac{\sum_i (\bar{y}_i - \bar{y})^2} {a - 1} \right]$$. This is the variance between groups with respect to the grand average.   

If the null hypothesis is true, then the within-group variance should be roughly the same as the among-group variance, but if we reject the null hypothesis, then the among-group variance should be larger [2]. 

To perform the actual statistical test, we compute the F statistic which is a ratio of the among-group variance to the within-group variance, computed using mean squares, specifically [2]: 

$$F = \frac{ \text{among-MS}}{ \text{within-MS}}$$

We compare the F statistic with the corresponding critical value to decide to reject or accept the null hypothesis (this is a one-sided test because we will only reject the null hypothesis if the $$\text{among-MS}$$ is greater than the $$\text{within-MS}$$) [2]. 

## Z-test

The z-test is similar in concept to the t-test presented above, with the main difference being that the z statistic is assumed to be normally distributed [3]. This test also requires that we know the variance of the population from which we drew samples, which in practice is rare so this test is not used very often [3]. The z-test also does not have different critical values determined by sample size, unlike the t-test [3]. Based on these facts, I will forego the details of the calculation, assuming that in practice I would probably use the t-test instead for this particular application. 

## Chi-Squared Test

Unlike the other tests that we’ve seen so far, a chi-squared test is a non-parametric test [1] that can also be used to compare samples from different groups [4]. This test is primarily used to analyze data contained in **contingency tables** for large sample sizes [4]. A contingency table is used to record the number of samples that have different categorical properties, for example to tabulate how many dogs and cats have spots or stripes [5]. We typically use a chi-squared test to determine if two of these categorical variables (like spots and stripes) are independent of each other [4]. I’m also not going to spend a lot of time detailing this test because I do not typically work with data of this kind in my research. 

Thanks for reading along as I explore some common statistical tests. 

## References

[1] Sirisilla, S. “7 Ways to Choose the Right Statistical Test for Your Research Study.” Enago.com. 4 Jan 2023. <https://www.enago.com/academy/right-statistical-test/> Visited 17 Feb 2023. 

[2] Dowdy, S., Wearden, S. Statistics for Research, Second Edition. John Wiley & Sons, 1991. 

[3] “Z-test.” Wikipedia. <https://en.wikipedia.org/wiki/Z-test> Visited 17 Feb 2023. 

[4] “Chi-squared test.” Wikipedia. <https://en.wikipedia.org/wiki/Chi-squared_test> Visited 17 Feb 2023. 

[5] “Contingency tables.” Wikipedia. <https://en.wikipedia.org/wiki/Contingency_table> Visited 17 Feb 2023. 
