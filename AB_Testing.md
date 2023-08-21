# A/B Testing

### Power vs. Statistical Significance vs. Effect Size ([link](https://meera.snre.umich.edu/power-analysis-statistical-significance-effect-size))

- Power → avoids a Type II error
    - In plain English, statistical power is the likelihood that a study will detect an effect when there is an effect there to be detected. If statistical power is high, the probability of making a Type II error, or concluding there is no effect when, in fact, there is one, goes down
- Stat.sig → avoids a Type I error

To understand power, it is helpful to review what inferential statistics test. When you conduct an inferential statistical test, you are often comparing two hypotheses:

- ***The null hypothesis*** – This hypothesis predicts that your program will not have an effect on your variable of interest. For example, if you are measuring students’ level of concern for the environment before and after a field trip, the null hypothesis is that their level of concern will remain the same.
- ***The alternative hypothesis*** – This hypothesis predicts that you will find a difference between groups. Using the example above, the alternative hypothesis is that students’ post-trip level of concern for the environment will differ from their pre-trip level of concern.

Statistical tests look for evidence that you can reject the null hypothesis and conclude that your program had an effect. **With any statistical test, however, there is always the possibility that you will find a difference between groups when one does not actually exist. This is called a Type I error**. Likewise, it is possible that **when a difference does exist, the test will not be able to identify it. This type of mistake is called a Type II error**.

**Power refers to the probability that your test will find a statistically significant difference when such a difference actually exists**. In other words, **power is the probability that you will reject the null hypothesis when you should (and thus avoid a Type II error)**. It is generally accepted that power should be .8 or greater; that is, you should have an 80% or greater chance of finding a statistically significant difference when there is one.

- The point of power calculations like these is to find out **what sample size we need** for our A/B test, how many views or users or form submissions or other interactions we need in each group to achieve the necessary power for our test. For more complicated tests, we on the data team sometimes **run simulations for power calculations**.
- “We realized that we couldn’t standardize power calculations across all tests. Some parts of our funnel were highly optimized and converted well, which meant we needed smaller samples sizes to detect the same effect we would want to see in an area that didn’t convert as well,” Des says. “Other areas had higher volume, like page views, but did not convert at well. While higher volume helps us reach the sample size need faster, we needed a larger effect size for the change to make an impact.” - similar to how P2P vs. GVC

**What is statistical significance?**

There is always some likelihood that the changes you observe in your participants’ knowledge, attitudes, and behaviors are due to chance rather than to the program. Testing for statistical significance helps you learn **how likely it is that these changes occurred randomly and do not represent differences due to the program**.

To learn whether the difference is statistically significant, you will have to compare the probability number you get from your test (the p-value) to the critical probability value you determined ahead of time (the alpha level). If the p-value is less than the alpha value, you can conclude that the difference you observed is statistically significant.

**P-Value**: the probability that the results were due to chance and not based on your program. P-values range from 0 to 1. The lower the p-value, the more likely it is that a difference occurred as a result of your program.

- P-value of 1-tail = P-value of 2-tail / 2 . It is because in two tail there are 2 critical regions

**Alpha (α) level**: the error rate that you are willing to accept. Alpha is often set at .05 or .01. The alpha level is also known as the Type I error rate. An alpha of .05 means that you are willing to accept that there is a 5% chance that your results are due to chance rather than to your program.

**What is effect size?**

When a difference is statistically significant, it does not necessarily mean that it is big, important, or helpful in decision-making. It simply means you can be confident that there is a difference. Let’s say, for example, that you evaluate the effect of an EE activity on student knowledge using pre and posttests. The mean score on the pretest was 83 out of 100 while the mean score on the posttest was 84. Although you find that the difference in scores is statistically significant (because of a large sample size), the difference is very slight, suggesting that the program did not lead to a meaningful increase in student knowledge.

- Standard error: It’s commonly defined as the standard deviation of the mean of a population. In simpler words it is an estimate of how far the sample mean is likely to be from the population mean. In an ideal world, you want a very low standard error.
- **A confidence interval is an interval in which a measurement or trial falls corresponding to a given probability**. Usually, the confidence interval of interest is symmetrically placed around the mean.
    - As the confidence level increases the margin of error also increases
    - Alpha is the portion of confidence interval that will not contain the population parameter α = 1 – CL

**To know if an observed difference is not only statistically significant but also important or meaningful**, you will need to calculate its effect size. Rather than reporting the difference in terms of, for example, the number of points earned on a test or the number of pounds of recycling collected, effect size is standardized. In other words, all effect sizes are calculated on a common scale -- which allows you to compare the effectiveness of different programs on the same outcome.

**How do I calculate effect size?**

There are different ways to calculate effect size depending on the evaluation design you use. **Generally, effect size is calculated by taking the difference between the two groups (e.g., the mean of treatment group *minus* the mean of the control group) and dividing it by the standard deviation of one of the groups**. For example, in an evaluation with a treatment group and control group, effect size is the difference in means between the two groups divided by the standard deviation of the control group.

To interpret the resulting number, most social scientists use this general guide developed by Cohen:

- < 0.1 = trivial effect
- 0.1 - 0.3 = small effect
- 0.3 - 0.5 = moderate effect
- > 0.5 = large difference effect

---

### How long do you run an experiment for?

([reference](https://medium.com/airbnb-engineering/experiments-at-airbnb-e2db3abf39e7))

- The problem with the naive method of using of the p-value as a stopping criterion is that the statistical test that gives you a p-value assumes that you designed the experiment with a sample and effect size in mind. If you continuously monitor the development of a test and the resulting p-value, you are very likely to see an effect, even if there is none. Another common error is to stop an experiment too early, before an effect becomes visible.
- But the most important reason is that you are performing a statistical test every time you compute a p-value and the more you do it, the more likely you are to find an effect.
- How long should experiments run for then? To prevent a false negative (a Type II error), **the best practice is to determine the minimum effect size that you care about and compute, based on the sample size (the amount of new samples that come every day) and the certainty you want, how long to run the experiment for**, before you start the experiment. [Here](http://www.evanmiller.org/ab-testing/sample-size.html) is a resource that helps with that computation.

---

### When NOT to run A/B tests

- Privacy concern
- Product thinking is critical here. Sometimes a change is obviously better UX but the test would take months to be statistically significant. If we are confident that the change aligns with our product strategy and creates a better experience for users, we may forgo an A/B test. In these cases, we may take qualitative approaches to validate ideas such as running usability tests or user interviews to get feedback from users. It’s a judgement call. If A/B tests aren’t practical for a given situation, we’ll use another tool in the toolbox to make progress. Our goal is continuous improvement of the product. In many cases, A/B testing is just one part of our approach to validating a change.
- When the causal action to be tested is not under the control of the organization (e.g. understanding user behavior change when they switch from iphone to android; paying people to switch biases the results)
- When there are too few units (e.g. establishing the counter-factual to a Merger and Acquisition event is extremely hard)
- When establishing a Control may incur too large an opportunity cost since they don’t receive the treatment (e.g. randomized experiments can be costly for rare events, such as understanding the impact of running ads during the Superbowl; or when the desired metric takes too long to measure, such as returning to a website to purchase a new car five years after the current car purchase)
- when the change is expensive relative to the perceived value (e.g. understanding the impact of forcibly signing out all users; studying what if we don’t display ads on Google at all)
- When the desired unit of randomization cannot be properly randomized (e.g understanding on the TV viewers level for TV ads)
- When what is being tested is unethical or illegal

When this is the case, often the best approach is to estimate the effects using multiple methods, incl. small-scale user experience studies, surveys, and **observational studies**. 

### O**bservational studies techniques**

- challenges: (1) how to construct control and test groups for comparisons; (2) how to model the impact given the groups
- **Interrupted Time Series**: a quasi-experiment design, where we can control the change within the system (*when* they experience the change) but not randomize the treatment.  Effects of the intervention are evaluated by changes in the level and slope of the time series and statistical significance of the intervention parameters
    - common issue: confounding effect - e.g. seasonality;
- interleaved experiments (good when there’s two homogenous changes)
- regression discontinuity design
- **instrumented variables** (IV) and **Natural Experiments**: IV is a technique that tries to approximate random assignment. Specifically, the goal is to identify an Instrument that allows us to approximate random assignment (this happens organically in a natural exp)
    - e.g. charter school seats are allocated by lottery and can thus be a good IC for some studies
    - [https://quantifyinghealth.com/examples-of-instrumental-variables/](https://quantifyinghealth.com/examples-of-instrumental-variables/)
- **Propensity Score Matching**: match users on a single constructed score; the idea is to build comparable Control and Treatment populations, often by segmenting the users by common confounds, and ensure the comparison between Control and Treatment is not due to population mix changes (or demographic difference in the population)
    - the key concern is that only observed covariates are accounted for; unaccounted factors may result in hidden biases. (”only work under ‘strong ignorability’ conditions”)
- **Difference in Difference**: difference in test pre-post, minus difference in control pre-post
    - assuming **no spill-over effect**; relies on **equal trends assumption**, or rather the assumption that no time-varying differences exist between the treatment and control groups (whether the outcome trend moves in parallel before the program began)
    - vs. Interrupted Time Series (ITS): The D-I-D focuses on the difference in means between groups or the rate ratios for only one measure each in the pre and post periods; Instead of a single measure pre and post, ITS requires multiple evenly spaced measures; ITS fits a model via least squares
- Common pitfalls:
    - lots of confounding variables, especially those unanticipated.
    - an unrecognized common cause: e.g. users of MS Office 365 who have more errors churn less. How? because they have high usage and are heavy users. Usage is the common cause here.
    - spurious or deceptive correlation.

---

### A/B Test Methodology

- Student’s independent (unpaired) two-sample t-test. This one is suitable to our situation since the two populations are distinct: the treatment and the control group are composed of different individuals, and the two sets are disjoint.
    - under the assumption that the data used for evaluation is drawn from a normal distribution for a large enough sample ([Central Limit Theorem](https://en.wikipedia.org/wiki/Central_limit_theorem)).
    - in A/B testing you never know the population mean, you’re estimating it, so always use the t-test. For N > 100 , the t-test numerically yields the same results as the z-test.
    
    ---
    

### Common A/B Test Pitfalls

**Biases**: In the sampling process, there are three types of biases, which are:

- Selection bias
- Under coverage bias
- Survivorship bias

Repeated significance testing always increases the rate of false positives, that is, you’ll think many insignificant results are significant (but not the other way around). [https://www.evanmiller.org/how-not-to-run-an-ab-test.html](https://www.evanmiller.org/how-not-to-run-an-ab-test.html)

- The problem will be present if you ever find yourself “peeking” at the data and stopping an experiment that seems to be giving a significant result. The more you peek, the more your significance levels will be off.
- the best way to avoid repeated significance testing errors is to not test significance repeatedly. Decide on a sample size in advance and wait until the experiment is over before you start believing the “chance of beating original” figures that the A/B testing software gives you. **“Peeking” at the data is OK as long as you can restrain yourself from stopping an experiment before it has run its course.**
- Can also be mitigated by: sequential A/B test ([link](https://www.evanmiller.org/sequential-ab-testing.html))
    - At the beginning of the experiment, choose a sample size 𝑁
    - Assign subjects randomly to the treatment and control, with 50% probability each.
    - Track the number of incoming successes from the treatment group. Call this number 𝑇 .
    - Track the number of incoming successes from the control group. Call this number 𝐶
    - If 𝑇−𝐶 reaches √(2N) , stop the test. Declare the treatment to be the winner.
    - If 𝑇+𝐶 reaches 𝑁 , stop the test. Declare no winner.
    - The two-sided test is essentially the same, but with an alternate ending: If 𝑇−𝐶 reaches √(2.25N) , stop the test. Declare the treatment to be the winner. If 𝐶−𝑇 reaches √(2.25N) , stop the test. Declare the control to be the winner. If 𝑇+𝐶 reaches 𝑁, stop the test. Declare no winner.
- Can also be mitigated by: Bayesian A/B test

## A/B Test in an online marketplace

network effect; changes to one side can affect the experience of the other side; cannibalization + spillover. 

- reference 1: [https://tech.ebayinc.com/research/the-design-of-a-b-tests-in-an-online-marketplace/](https://tech.ebayinc.com/research/the-design-of-a-b-tests-in-an-online-marketplace/)

Mitigations: 

- Country test
- Cluster-based test
- Category-level test (ebay) paired with variance reduction (difference in difference, or post-stratification - using pre-experiment data as a covariate in regression models)