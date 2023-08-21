# Applied Stats

Backbone of most DS interviews; below notes are mostly from "Practical Stats for Data Science". 

## Basic Concepts for Exploratory Data Analyses (EDA)

- **Robust** = not sensitive to extreme values (trimmed means and median are more robust than just simple averages)
- “statisticians estimate, data scientists measure”
    - estimate: from a sample, key to have uncertainty
    - measure: from the whole population
- “**Location**” (1st moment)
    - Truncated Mean = capped average = trimmed mean = the average of numbers after dropping extreme values (could be fixed number of extreme values, or not fixed number but fixed by bounds)
    - Mean, Median
    - **Mean vs. Median** (when to use which): [https://towardsdatascience.com/mean-or-median-choose-based-on-the-decision-not-the-distribution-f951215c1376](https://towardsdatascience.com/mean-or-median-choose-based-on-the-decision-not-the-distribution-f951215c1376) \
        - *The choice to use a mean or a median should be primarily driven by the goals of the analysis. When the underlying business decision depends on a total (e.g. total revenue or total sales), the mean is usually the better metric because, unlike the median, it has a direct relationship with the total. Means are sensitive to extreme values, so care should be taken to ensure that they are calculated on clean, meaningful datasets.*
        - *When the distribution is skewed, the median can provide a more intuitive sense of a “typical” value, but this does not necessarily mean that it is the basis of the optimal decision-making policy.*
- **Variability** = dispersion = whether the data values are tightly clustered or spread out; 2nd moment
    - **deviations**: **errors** / **residuals**; the difference between observed value and the estimate of location
    - **variance** = s^2 = mean squared error = sum of squared deviations from the mean divided by n-1
    - **standard deviation (s.d.)** = s = square root of variance = L-2 norm = Euclidean norm
    - Degree of freedom, or N or N-1?
        - The distinction is usually not important since n is generally large enough that it won’t make much difference
        - It is based on the premise that, you want to make estimates about a population based on a sample.
        - If you use the intuitive denominator of n in the variance formula, you will underestimate the true value of the variance and the s.d. in the population - which leads to a biased estimate. Using n-1, the s.d. become an unbiased estimate. But why?
        - It’s because of degree of freedom, which takes into account the number of constraints in computing an estimate. **In this case, there are n-1 degree of freedom since there is ONE constraint: the s.d. depends on calculating the sample mean.**
    - mean absolute deviation = the mean of the absolute values of deviation from the mean = L-1 norm = Manhattan Norm
    - Neither the variance, the s.d. nor the mean absolute deviation is robust to outliers and extreme values.
    - Interquartile range (IQR): difference between the 25th percentile and the 75th percentile.
- **Skewness**: 3rd moment
    - whether the data is skewed/leaning towards larger or smaller values; Where one tail of a distribution is longer than the other.
    - Tail
        - The long narrow portion of a frequency distribution, where relatively
        extreme values occur at low frequency
- **Kurtosis**: 4th moment
    - indicates the propensity of the data to have extreme values
- Density plot vs. histogram: a density plot corresponds to plotting the histogram as a proportion rather than counts; smoothed histogram, on a continuous line
    - requires a function to estimate a plot based on data (kernel density estimate)
- **Mode**:
    - The most commonly occurring category or value in a data set.
- **Correlation**:
    - Variables X and Y (each with measured data) are said to be positively correlated if high values of X go with high values of Y, and low values of X go with low values of Y. If high values of X go with low values of Y, and vice versa, the variables are negatively correlated.
    - **correlation coefficient**: an estimate of the correlation between two variables that always lies on the same scale (-1 to +1) (0 indicates no correlation)
    - To compute: **Pearson’s correlation coefficient -** we multiply **deviations** from the mean for variable 1 times those for variable 2, and divide by the product of the standard deviations
    - Like the mean and standard deviation, the correlation coefficient is sensitive to outliers in the data (can use trimming to remove outliers)

## Data and Sampling Distributions

- **Sample**: a subset from a large data set (population)
    - **sampling procedure**:
        - **random** sampling: Drawing elements into a sample at random.
        - **stratified** sampling: Dividing the population into strata and randomly sampling from each strata.
        - simple random sampling: The sample that results from random sampling without stratifying the population. each available member of the population being sampled has an equal chance of being chosen for the sample at each draw.
        - sample bias: A sample that misrepresents the population.
        - **self-selection bias**—the people motivated to write reviews may be those who had poor experiences, may have an association with the establishment, or may simply be a different type of person from those who do not write reviews.
        - **Selection bias** refers to the practice of selectively choosing data—consciously or unconsciously—in a way that that leads to a conclusion that is misleading or ephemeral.
            - Specifying a hypothesis, then collecting data following randomization and random sampling principles, ensures against bias.
        - **Data quality** in data science involves **completeness**, **consistency of format**, **cleanliness**, and **accuracy of individual data points**. Statistics adds the notion of **representativeness**.
- **Errors** due to random chance, vs. errors due to bias:
    - An unbiased process will produce error, but it is random and does not tend strongly in any direction
    - Statistical bias refers to measurement or sampling errors that are systematic and produced by the measurement or sampling process. Bias = systematic error
- Regression to the mean: extreme observations tend to be followed by more central ones. a form of selection bias
- **Sample statistic**: A metric calculated for a sample of data drawn from a larger population
- **Standard error**: the variability (s.d.) of a sample statistic over many samples (not to be confused with standard deviation which, by itself, refers to variability of individual data values). A single metric that sums up the variability in the sampling distribution for a statistic.

$$
SE = {{S}\over{\sqrt{n}}}
$$

- **Central Limit Theorem (CLT)**: the tendency of the *sampling* distribution to take on a normal shape as sample size rises. (i.e. if you sample many times, and you look at all the means you have calculated, these means will likely be more regular and bell-shaped than the distribution of the data itself). **The larger the sample that the statistic is based on, the more this is true. The larger the sample, the narrower the distribution of the sample statistic.**
    - CLT needs the sample size to be large enough and the departure of the data from normality to be not too great
    - CLT allows normal-approximation formulas like the t-distribution to be used in calculating sampling distributions for inference - that is, confidence intervals and hypothesis tests.
- The **Bootstrap**:
    - draw additional samples, with replacement, from the sample itself and recalculate the statistic or model for each resample.
    - In this way we effectively create an infinite population in which the probability of an element being drawn remains unchanged from draw to draw.
    - used to quickly assess the variability of a sample statistic. a general tool **that can be used to generate confidence intervals (below)** for most statistics, or model parameters.
    - **Bagging: bootstrap aggregation**, i.e. running multiple models (e.g. trees) on bootstrap samples and then average predictions
    - The bootstrap does not compensate for a small sample size; it does not create new data, nor does it fill in holes in an existing data set. It merely informs us about how lots of additional samples would behave when drawn from a population like our original sample.
- **Confidence Interval**: the percentage of CI, constructed in the same way from the same population, expected to contain the statistic of interest. Generally, **an x% C.I. around a sample estimate should, on average, contain similar sample estimates x% of the time.**
    - the higher the level of confidence, the wider the interval. Also, the smaller the sample, the wider the interval.
    - “If i randomly draw a sample, x% CI is the chance that the sample statistic falls within the CI range” “Given a sampling procedure and a population, what is the probability that…”
- **Normal Distribution**:
    - **Standardize**: Subtract the mean and divide by the standard deviation.
    - A **standard normal distribution** is one in which the units on the x-axis are expressed in terms of standard deviations away from the mean.
        - To convert data to **z-scores**, you subtract the mean of the data and divide by the standard deviation; you can then compare the data to a normal distribution.
        - Converting data to z-scores (i.e., standardizing or normalizing the data) does not make the data normally distributed. It just puts the data on the same scale as the standard normal distribution, often for comparison purposes.
    - caution 🛎️: most of the variables used in a typical data science project - in fact most raw data as a whole - are not normally distributed; they usually have long tails (**power law**). The utility of the normal distribution derives from the fact that many statistics are normally distributed in their sampling distribution (CLT). (”While raw data is typically not normally distributed, errors often are, as are averages and totals in large samples.”)
    - an **error** is the difference between an actual value and a statistical estimate like the sample mean.
    - A **QQ-Plot** is used to visually determine how close a sample is to the normal distribution. The QQ-Plot orders the z-scores from low to high, and plots each value’s z-score on the y-axis; the x-axis is the corresponding quantile of a normal distribution for that value’s rank. Since the data is normalized, the units correspond to the number of standard deviations away of the data from the mean. If the points roughly fall on the diagonal line, then the sample distribution can be considered close to normal.
    - related - How to identify normal distribution:
        1. check if mean = median
        2. histogram
        3. boxplot
        4. QQ plot
        5. K-S test (Kolmogorov-Smirnov test): computes distance between empirical distribution and theoretical distribution. If it’s a normal distribution, KS statistic = 0; p-value can be used
        6. Shapiro-Wilk Test: can only be used for normal distribution; use p-value
- **Student’s t-Distribution:** used as a reference basis for the distribution of sample means, differences between two sample means, regression parameters, and more.
    - a normally shaped distribution but a bit thicker and longer on the tails.
    - Distributions of sample means are typically shaped like a t-distribution
    - The larger the sample, the more normally shaped the t-distribution becomes.
- **Binomial Distribution**: binary, Having two outcomes. Distribution of number of successes in x trials.
    - flip a coin; buy/don’t buy; click/don’t click, survive/die, and so on.
    - The binomial distribution is the frequency distribution of the number of successes (x) in a given number of trials (n) with specified probability (p) of success in each trial.
    - Mean: np;
    - variance: np(1-p);
    - With a large enough number of trials and when p is close to 0.5 (provided p is not too close to 0 or 1), the binomial distribution is virtually indistinguishable from the normal distribution.
- **Poisson distribution**: many processes produce events randomly at a given overall rate (e.g. visitors arriving at a website; “How much capacity do we need to be 95% sure of fully processing the internet traffic that arrives on a server in any 5- second period?”). The frequency distribution of the number of events in sampled units of time or space.
    - **Exponential distribution**: The frequency distribution of the time or distance from one event to the next event. (e.g. time between visits to a website)
    - **Weibull distribution**: A generalized version of the exponential, in which the event rate is allowed to shift over time. If, however, the event rate changes over the time of the interval, the exponential (or Poisson) distributions are no longer useful. This is likely to be the case in mechanical failure—the risk of failure increases as time goes by. The Weibull distribution is an extension of the exponential distribution, in which the event rate is allowed to change, as specified by a shape parameter, beta.
    - **Lambda $\lambda$**: The rate (per unit of time or space) at which events occur. the **mean** number of events that occurs in a specified interval of time or space. The **variance** for a Poisson distribution is also **Lambda $\lambda$.**
    - key assumption: lambda remains constant, which is rarely reasonable in a global sense (but workable when time or space is divided into segments that are sufficiently homogeneous so that analysis or simulation within those periods is valid).
- Summary: Random selection of data can reduce bias and yield a higher quality data set than would result from just using the conveniently available data. Knowledge of various sampling and data generating distributions allows us to quantify potential errors in an estimate that might be due to random variation. At the same time, the bootstrap (sampling with replacement from an observed data set) is an attractive “one size fits all” method to determine possible error in sample estimates.

## Statistical Experiments and Significance Testing

- The goal is to design an experiment in order to confirm or reject a hypothesis
- **inference**: reflects the intention to apply the experiment results, which involve a limited set of data, to a larger process or population.
- **Test statistic**: The metric used to measure the effect of the treatment.
- **Hypothesis Testing**: helps to see if the observed effect is due to random chance. Statistical hypothesis testing was invented as a way to protect researchers from being fooled by random chance.
    - Null hypothesis: The hypothesis that chance is to blame.
    - two-way vs. one-way: two-way is more conservative (changes can be in both directions)
- Resampling: repeatedly sample values from observed data, with a general goal of assessing random variability in a statistic.
    - bootstrap: with replacement; used to assess the reliability of an estimate
    - **permutation test** and procedure: used to test hypotheses; The procedure of **combining two or more samples together**, and randomly (or exhaustively) reallocating the observations to resamples. = Randomization test, random permutation test, exact test. Permutation tests are useful heuristic procedures for exploring the role of random variation; assumption about data normality is not needed
        - two or more samples are involved, typically the groups in an A/B or other hypothesis test.
        - The first step in a permutation test of a hypothesis is to combine the results from groups A and B (and, if used, C, D, ...) together. This is the logical embodiment of the null hypothesis that the treatments to which the groups were exposed do not differ.
        - Shuffle the combined data, then randomly draw (without replacing) a
        resample of the same size as group A.
        - From the remaining data, randomly draw (without replacing) a resample of
        the same size as group B.
        - calculate the statistic or estimate; repeat R times to yield a permutation distribution of the test statistic.
        - Now go back to the observed difference between groups and compare it to the set of permuted differences. If the observed difference lies well within the set of permuted differences, then we have not proven anything—the observed difference is within the range of what chance might produce. However, if the observed difference lies outside most of the permutation distribution, then we conclude that chance is not responsible. In technical terms, the difference is **statistically significant**.
- **statistical significance**: whether this difference is within the range of what random chance might produce (an effect more extreme than what chance might produce)
- **p-value**: the probability of obtaining results as unusual or extreme as the observed results (e.g. 0.05, only 5% of the time *could* chance produce a result this extreme). When p-value falls short of statistical significance, what we are really saying is that “effect not proven” - it could be that larger samples would yield smaller p-values.  p-values are sometimes used as intermediate inputs in some statistical or machine learning models—a feature night be included in or excluded from a model depending on its p-value.
- **alpha**: the probability threshold of “extreme-ness/unusual-ness” that chance results must surpass, for actual outcomes to be deemed statistically significant. e.g. 5% when p=0.05
- **type I error**: mistakenly conclude an effect is real when it is due to chance. Statistical tests are used to primarily to defend from being fooled by random chance, and thus Type I errors.
- **type II error**: mistakenly conclude an effect is not real, due to chance, when it is real
- **t-test**: statisticians found that a good approximation to the permutation distribution was the t-test. It is used for the very common two-sample comparison - A/B test (when resampling thousands or more times was not practical). A test statistic could be standardized and compared to the reference distribution.
- **False discovery rate (FDR)**: across multiple tests, the rate of making a Type I error.
    - for predictive modeling, use **Cross-Validation** and **a Holdout sample** to mitigate **Multiplicity**. Multiplicity in a research study or data mining project (multiple comparisons, many variables, many models, etc.) increases the risk of concluding that something is significant just by chance.
- the **Bonferroni adjustment**, simply divides the alpha by the number of observations n.
- **Multicollinearity error**: regression algorithms choke if exactly redundant predictor variables are present (**degrees of freedom**).
    - When the predictor variables have perfect, or near-perfect, correlation, the regression can be unstable or impossible to compute.
- **two groups: t-test; 3 or more groups: F-statistic** (based on the ratio of the variance across group means to the variance due to residual error; the higher the ratio, the more statistically significant the result).
- **ANOVA = analysis of variance**: the statistical procedure that tests for a statistically significant difference among the groups. (instead of A/B, we now have multiple groups).
    - omnibus test: A single hypothesis test of the overall variance among multiple group means.
        - Combine all the data together in a single box
        - Shuffle and draw out four resamples of five values each (e.g. 4 groups)
        - Record the mean of each of the fourgroups
        - Record the variance among the four group means
        - Repeat steps 2–4 many times (say1,000)
        - p-value is then the proportion of the time the resampled variance exceeded the observed variance.
    - pairwise comparison: A hypothesis test (e.g., of means) between two groups among multiple groups.
        - the more such pairwise comparisons, the more likely we will be fooled by chance.
- **F-statistic**: A standardized statistic that measures the extent to which differences among group means exceeds what might be expected in a chance model.
    - based on the ratio of the variance across group means (i.e. the treatment effect) to the variance due to residual error. the higher this ratio, the more statistically significant the result.
- **Chi-Squared Test**: used with count data to test how well it fits some expected distribution. often used to assess whether the null hypothesis of independence among variables is reasonable.  goes beyond A/B testing and tests multiple treatments at once.
    - e.g. three different web banners, can test whether the click rates differ to an extent greater than chance might cause. the null hypothesis assumption that all three banners share the same click rate.
    - The chi-square distribution is the reference distribution (which embodies the assumption of independence) to which the observed calculated chi- square statistic must be compared.
    - One data science application of the chi-square test, especially Fisher’s exact version, is in determining appropriate sample sizes for web experiments.
- Multi-arm bandits: an analogy for a multitreatment experiment. Pull the winning arm more, the perceived losing arms less.
    - thompson sampling is a way to do it
- **Power**: the probability of detecting a given effect size, with a given sample size and variability.
    - If you run a web test, how do you decide how long it should run (i.e., how many
    impressions per treatment are needed)? there is no good general guidance—it depends, mainly, on the frequency with which the desired goal is attained. \
    - 4 moving parts (Specify any three of them, and the fourth can be calculated. Most commonly, you would want to calculate sample size, so you must specify the other three.)
        - **sample size**
        - **effect size** you want to detect (based on business ROI calc, maybe)
        - **significance level (alpha)** at which the test will be done (e.g. 0.05)
        - **power** (e.g. 80%)
    - **effective size:** The minimum size of the effect that you hope to be able to detect in a statistical test
- **Sample size**: how big of a sample you need, usually estimated from the power calculation.
- Significance level: The statistical significance level at which the test will be conducted.

## **Regression and Prediction**

- Regression: while correlation measures the strength of an association between two variables, regression quantifies the nature of the relationship.
    - Regression coefficient: The slope of the regression line.
    - **Least squares**: The method of fitting a regression by minimizing **the sum of squared residuals (RSS, residual sum of squares)**.
        - Why do statisticians differentiate between the estimate and the true value? The estimate has uncertainty, whereas the true value is fixed.
        - sensitive to outliers
- **Simple Linear Regression**: $Y = b_0 + b_1X + e$ ; b_0 is the intercept, b_1 is the slope (coefficient) for X. e is error
- Multiple linear regression: Instead of a line, we now have a linear model—the relationship between each coefficient and its variable (feature) is linear.
    - **Root mean squared error**: The square root of the average squared error of the regression (this is the most widely used metric to compare regression models).
    
    $$
    \text{RMSE}(y, \hat{y}) = \sqrt{\frac{\sum_{i=1}^{N} (y_i - \hat{y}_i)^2}{N}}
    $$
    
    - **R-squared**: coefficient determination; the proportion of variance explained by the model, from 0 to 1.
        - c.f. **residual standard error,** or **RSE:** The same as the root mean squared error, but adjusted for degrees of freedom (denominator, as opposed to number of records).  with p predictors,
    
    $$
    \text{RSE}(y, \hat{y}) = \sqrt{\frac{\sum_{i=1}^{N} (y_i - \hat{y}_i)^2}{N-p-1}}
    $$
    
- **Adjusted R-squared**: adjusts for the degree of freedom
- Including additional variables always reduces RMSE and increases R^2. But it runs the risk of overfitting
    - can use **AIC** (penalizes adding terms to a model); goal is to minimize AIC
    - use stepwise regression
    - **penalized regression** uses the same idea; Rather than eliminating predictor variables entirely—as with stepwise, forward, and backward selection—penalized regression applies the penalty by reducing coefficients, in some cases to near zero. Common penalized regression methods are **ridge regression** and **lasso regression**.
- The higher the t-statistic (and the lower the p-value), the more significant the predictor.
- **Cross-validation**: k-fold cross-validation, extends the idea of a holdout sample to multiple sequential holdout samples.
- **prediction interval** vs. confidence interval: A prediction interval pertains to uncertainty around a single value, while a confidence interval pertains to a mean or other statistic calculated from multiple values. Thus, a prediction interval will typically be much wider than a confidence interval for the same value. Using a confidence interval when you should be using a prediction interval will greatly underestimate the uncertainty in a given predicted value.
- **Confounding variables**: An important predictor that, when omitted, leads to spurious relationships in a regression equation.
- **Interactions**: An interdependent relationship between two or more predictors and the response.
- **Correlated Predictors**: In multiple regression, the predictor variables are often correlated with each other. Having correlated predictors can make it difficult to interpret the sign and value of regression coefficients (and can inflate the standard error of the estimates).
- **Interactions** and **Main Effects**: Statisticians like to distinguish between main effects, or independent variables, and the interactions between the main effects. Main effects are what are often referred to as the **predictor variables** in the regression equation. An implicit assumption when only main effects are used in a model is that the relationship between a predictor variable and the response is independent of the other predictor variables. This is often not the case.
    - tree models automatically search for optimal interaction terms
    - An interaction term between two variables is needed if the effect of one variable depends on the level of the other.
- **Standardized residuals**: residuals divided by the standard error of the residuals; the number of standard errors away from the regression line.
    - to detect **outliers**
    - “too far” sometimes means “more than 1.5 the inter-quartile range”
- **Heteroskedasticity**: When some ranges of the outcome experience residuals with higher variance (may indicate a predictor missing from the equation). the lack of constant residual variance; errors are greater for some portion of the range than others. may suggest an incomplete model.
    - Statisticians may also check the assumption that the errors are independent. This is particularly true for data that is collected over time.
- Cook’s distance and hat-value can be used to understand influential points and their leverage. An **influence plot** or **bubble plot** combines standardized residuals, the hat-value, and Cook’s distance in a single plot.
- Nonlinear regression:
    - polynomial: Adds polynomial terms (squares, cubes, etc.) to a regression.
    - spline: Fitting a smooth curve with a series of polynomial segments.
    - knots: Values that separate spline segments.
    - Generalized additive models: Spline models with automated selection of knots.
- Summary: In classical statistics, the emphasis is on finding a good fit to the observed data **to explain or describe** some phenomenon, and the strength of this fit is how traditional (“in-sample”) metrics are used to assess the model. In data science, by contrast, the goal is typically **to predict** values for new data, so metrics based on predictive accuracy for out-of-sample data are used. Variable selection methods are used to reduce dimensionality and create more compact models.

## Classification

- **Naive bayes**: uses the probability of observing predictor values, given an outcome, to estimate the probability of observing outcome Y = i, given a set of predictor values.
    - we assume X_i is independent of all other X_k (for k =/= i)
    - when a predictor category is absent in the training data, the algo assigns zero probability to the outcome variable in new data, rather than simply ignoring this variable and using information from other variables like other models.
    - produce biased estimates.
- **Discriminant analysis**: the earliest statistical classifier (OG); can provide a measure of predictor importance (when predictor variables are normalized, the discriminator weights are measure of importance), used as a computationally efficient method for feature selection; discriminant analysis assumes the predictor variables are normally distributed continuous variables, but, in practice, the method works well even for non-extreme departures from normality, and for binary predictors.
    - **linear discriminant analysis**, or **LDA**: focuses on maximizing the “between” sum of squares (variation between two groups) relative to the “within” sum of squares (variation within group). this yields the greatest separation between the two groups. Using the **covariance matrix**, it calculates a **linear discriminant function**, which is used to distinguish records belonging to one class from those belonging to another.
        - The only limiting factor is the number of records (estimating the covariance matrix requires a sufficient number of records per variable, which is typically not an issue in data science applications).
    - **covariance**: measures the relationship between two variables X and Z; like correlation, positive values indicate a positive relationship and negative values indicate a negative relationship; however correlation is in [-1,1], covariance is on the same scale as the variables. $cov_{x,z}=\frac{\sum_{i=1}^{N}(x_{i}-\bar{x})(z_{i}-\bar{z})}{N-1}$
- **Logistic Regression**: one of the most commonly used ones; analogous to multiple linear regression, except the outcome is binary. Like discriminant analysis, it’s a structured model approach rather than a data-centric approach (i.e. KNN, Naive Bayes). Computes fast; it produces a model that can be scored to new data rapidly, without re-computation. a special instance of a **generalized linear model (GLM)**
    - **Logit**: The function that maps the probability of belonging to a class with a range from ± ∞ (instead of 0 to 1). ******Log odds.******  we can map to a class label by applying a cutoff rule —any record with a probability greater than the cutoff is classified as a 1.
    - **logistic response function**: in which we map a probability (which is on a 0–1 scale) to a more expansive scale suitable for linear modeling. using an **inverse logit**; to ensure response stays between 0 and 1.
    
    $$
    log(Odds(Y=1)) = \beta_0 + \beta_1 x_1 + \beta_2 x_2 + \beta_3 x_3 + ...
    $$
    
    - A lower cutoff is often needed if the goal is to identify members of a **rare class** (imbalance in labels)
    - fitting is different from linear regression - least squares are not applicable here. There is no closed form solutions here; model must be fit with **maximum likelihood estimation** (**MSE**) - a process to find the model that is most likely to have produced the data we see. In the fitting process, the model is evaluated using a metric called **deviance**; lower deviance = better fit.
    - 🔥 Key assumptions:
    - Assessing model: not using R-squared or RMSE, but more general metrics for classification (accuracy, precision, false positives, etc. see below)
- **Generalized linear model (GLM):** has two components:
    - a probability distribution of the family (binomial in the case of logistic regression); The poisson distribution is commonly used to model count data
    - a link function mapping the response to the predictor (logit in the case of logistic regression, mapping probability to infinite values)
- **Accuracy**: the percent (proportion) of cases identified correctly ((**true positives** + **true negatives**)/total sample size)
- **Precision**: The percent (proportion) of predicted 1s that are actually 1s. **TP/(FP+TP)**
    - **confusion matrix**: A tabular display (2×2 in the binary case) of the record counts by their predicted and actual classification status.
    - **False positive rate**: **FP/(FP+TN)** (divided by total number of negatives)
    - **False negative rate**: **FN/(FN+TP)** (divided by total number of positives)
- **Sensitivity** = **Recall**: The percent (or proportion) of **1s** correctly classified. **TP/(FN+TP)**; measures the strength of the model to predict a positive outcome
- **Specificity**: The percent (or proportion) of **0s** correctly classified.**TN/(FP+TN)**
- **ROC curve**: sensitivity vs specificity; there’s a tradeoff between recall and specificity; a good model with have a ROC curve that hugs the upper-left corner - it will correctly classify lots of 1s without misclassifying lots of 0s as 1s.
    - **AUC**: total area under the ROC curve. AUC = 1 indicates a perfect model. A completely ineffective model will have AUC = 0.5.
- Precision-Recall curve (PR curve): especially useful when labels are highly imbalanced
- **Strategies for Imbalanced Data:** Imbalanced data usually indicates that correctly classifying one class (the 1s) has higher value, and that value ratio should be built into the assessment metric.
    - **undersample** the prevalent class: assuming the dominant class has many duplicate records. in general, having tens of thousands of records for the less dominant class is enough. The more easily distinguishable the 1s are from the 0s, the less data needed.
        - One criticism of the undersampling method is that it throws away data and is not using all the information at hand, especially when data is limited.
    - **oversample** with rarer class by drawing additional rows with replacement (**bootstrapping**)
    - can achieve a similar effect by **weighting** the data. An easy way to change the loss function, discounting errors for records with low weights in favor of records with higher weights.
    - data generation, e.g. SMOTE algorithm: generates synthetic data

## Statistical Machine Learning

- **K-Nearest Neighbors**: for both classification (assigning it to the class that similar records belong to, by a majority vote) and regression (average of the K-Nearest Neighbors); **unsupervised learning**
    - Similarity (nearness) is determined using a distance metric, which is a function that measures how far two records (x1, x2, ... xp) and (u1, u2, ... up) are from one another.
        - Euclidean distance: $\sqrt{(x_1-u_1)^2 + (x_2-u_2)^2 + ...}$
        - Manhattan distance: $|x_1-u_1| + |x_2-u_2| + ...$ useful if similarity is defined as point-to-point travel time
        - Euclidean and Manhattan distance do not account for the correlation, effectively placing greater weight on the attribute that underlies those features. **Mahalanobis distance** is attractive since it accounts for the correlation between two variables. This is useful since if two variables are highly correlated, Mahalanobis will essentially treat these as a single variable in terms of distance. The downside of using Mahalanobis distance is increased computational effort and complexity; it is computed using the covariance matrix
    - can capture local structure in data
    - BIAS-VARIANCE TRADEOFF
- **Tree Models**: for both classification and regression; In contrast to regression and logistic regression, trees have the ability to discover hidden patterns corresponding to complex interactions in the data. usually easily interpretable.
    - Tree models provide a visual tool for exploring the data, to gain an idea of what variables are important and how they relate to one another. Trees can capture nonlinear relationships among predictor variables.
    - Tree models provide a set of rules that can be effectively communicated to nonspecialists, either for implementation or to “sell” a data mining project.
    - **Impurity**: The extent to which a mix of classes is found in a subpartition of the data (the more mixed, the more impure). Heterogeneity
        - It turns out that accuracy is not a good measure for impurity. Instead, two common measures for impurity are the **Gini impurity** and **entropy** or information
    - **recursive partitioning**: algorithm to construct a decision tree; data is repeatedly partitioned using predictor values that do the best job of separating the data into relatively homogeneous partitions
    - **Overfitting** - too many trees/branches. A simple and intuitive method of reducing tree size is to **prune** back the terminal and smaller branches of the tree, leaving a smaller tree. How far should the pruning proceed? A common technique is to prune the tree back to the point where the error on holdout data is minimized.
    - **Random Forest**: A type of bagged estimate based on decision tree models. in addition to sampling the records, the algorithm also samples the variables (more than simple bagging, also a bootstrap sampling of variables - rule of thumb is to choose sqrt(p) when p is number of predictor variables).
        - averaging (or taking majority votes) of multiple models—an ensemble of models—turns out to be more accurate than just selecting one
        - **Bagging**: bootstrap aggregating; form a collection of models by bootstrapping the data. each new model is fit to a bootstrap resample
        - **OOB**: The out-of-bag (OOB) estimate of error is the error rate for the trained models, applied to the data left out of the training set for that tree.
        - The random forest method is a “black box” method. It produces more accurate
        predictions than a simple tree, but the simple tree’s intuitive decision rules are
        lost. (tradeoff)
        - **Feature importance** / **variable importance**: random forest gives us this info
    - **Boosting**: requires more tuning; fits a series of models with each successive model fit to minimize the error of the previous models. e.g. Adaboost, **gradient boosting**, and stochastic gradient boosting. Updating weights in each round so that the weights are increased for the observations that were misclassfied.
        - **Regularization**: A technique to avoid overfitting by adding a penalty term to the cost function on the number of parameters in the model.
            - **Ridge** regression minimizes the sum of squared residuals plus a penalty on the number and size of the coefficients
            - **Lasso** is similar, except that it uses Manhattan distance instead of Euclidean distance as a penalty term

## **Unsupervised Learning**

- Clustering: segmentation
- Unsupervised learning techniques generally require that the data be appropriately **scaled**. This is different from many of the techniques for regression and classification in which scaling is not important (an exception is K-nearest neighbors).
    - Categorical data can pose a special problem for some clustering procedures. As with K-nearest neighbors, unordered factor variables are generally converted to a set of binary (0/1) variables using **one hot encoding**
- **Principal components analysis (PCA)** is a technique to discover the way in which numeric variables covary. to combine multiple numeric predictor variables into a smaller set of variables, which are weighted linear combinations of the original set. reducing the dimension of the data
    - Principal component: A linear combination of the predictor variables.
    - PCA can be viewed as the unsupervised version of linear discriminant analysis
- **K-Means Clustering**: a technique to divide data into different groups, where the records in each group are similar to one another. K-means divides the data into K clusters by minimizing the sum of the squared distances of each record to the mean of its assigned cluster. K-means does not ensure the clusters will have the same size, but finds the clusters that are the best separated.
    - need to standardize; if not, variables with large scale will dominate the clustering process
    - algorithm isn’t guaranteed to find the best possible solution (because it starts by randomly assigning points to clusters), hence it is recommended to run the algorithm several times using different random samples to initialize the algorithm.
    - **selecting K: the elbow method** - to identify when the set of clusters explains “most” of the variance in the data. Adding new clusters beyond this set contributes relatively little incremental contribution in the variance explained. The elbow is the point where the cumulative variance explained flattens out after rising steeply, hence the name of the method. harder to tell when variance explained goes gradually up.
        - In evaluating how many clusters to retain, perhaps the most important test is this: how likely are the clusters to be replicated on new data? Are the clusters interpretable, and do they relate to a general characteristic of the data, or do they just reflect a specific instance? You can assess this, in part, using cross- validation.
- **Hierarchical Clustering**: more flexible than K-means and more easily accommodates non-numerical variables. It is more sensitive in discovering outlying or aberrant groups or records; starts by setting each record as its own cluster and iterates to combine the least dissimilar clusters.
    - dendrogram
    - the agglomerative algorithm: Progressively, clusters are joined to nearby clusters until all records belong to a single cluster
    - Inter-cluster distances are computed in different ways, all relying on the set of all inter-record distances. There are four common measures of dissimilarity: complete linkage, single linkage, average linkage, and minimum variance.
- Model-based clustering: the multivariate normal distribution; Clusters are assumed to derive from different data-generating processes with different probability distributions.
    - each record is assumed to be distributed as one of K multivariate-normal distributions, where K is the number of clusters.
    - Each distribution has a different mean and covariance matrix
    - limitations: The methods require an underlying assumption of a model for the data, and the cluster results are very dependent on that assumption. The computations requirements are higher than even hierarchical clustering, making it difficult to scale to large data.

## Causal Inference

- Neyman-Rubin Causal Model: [https://scholar.princeton.edu/sites/default/files/jmummolo/files/po_model_jm.pdf](https://scholar.princeton.edu/sites/default/files/jmummolo/files/po_model_jm.pdf)
    - Fundamental Problem of Causal Inference: We cannot observe both potential outcomes. So how can we calculate τi = Y1i − Y0i?
    - Causal inference is difficult because it involves missing data.
    - Homogeneity is one solution:
        - If {Y1i , Y0i} is constant across individuals, then cross-sectional comparisons will recover τi
        - If {Y1i , Y0i} is constant across time, then before and after comparisons will recover τi
        - In social phenomena, unfortunately, homogeneity is very rare.
    - No interference assumption is an example of an exclusion restriction. We rely on outside information to rule out the possibility of certain causal effects (e.g. you taking the treatment has no effect on my potential outcomes).
    - Because τi are unobservable, we shift what we are interested in to: Average Treatment Effect (ATE) = Average of all treatment potential outcomes − Average of all control potential outcomes
        - Comparisons between observed outcomes of treated and control units can often be misleading, because Bias term unlikely to be 0 in most applications and Selection into treatment is often associated with the potential outcomes.
        - Since missing potential outcomes are unobservable we must make assumptions to fill them in, i.e. estimate missing potential outcomes. In the causal inference literature, we typically make assumptions about the assignment mechanism to do so. (e.g. random assignment)
- Experiment design: [https://statweb.stanford.edu/~owen/courses/363/stats-263-lecture-1.pdf](https://statweb.stanford.edu/~owen/courses/363/stats-263-lecture-1.pdf)
- In many circumstances, however, randomized experiments are not possible due to ethical or practical concerns. In such scenarios there is a non-random assignment mechanism. This is the case for the example of college attendance: people are not randomly assigned to attend college. Rather, people may choose to attend college based on their financial situation, parents' education, and so on. Many statistical methods have been developed for causal inference, such as [propensity score matching](https://en.wikipedia.org/wiki/Propensity_score_matching). These methods attempt to correct for the assignment mechanism by finding control units similar to treatment units.
- **Correlation = causation + coufounding effects**, and sometimes we can infer a lot about causation from correlation.
    - E.g. send latency and total message sends
