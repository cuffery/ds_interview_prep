# ML Knowledge Bank

From *Machine Learning Yearning:* 

- **Precision**: the fraction of images in the dev set it labeled as cats that are really cats
- **Recall**: percentage of all cat images in the dev set that it correctly labeled as a cat.
    - **ROC Curves** summarize the trade-off between the true positive rate and false positive rate for a predictive model using different probability thresholds.
    - **Precision-Recall curves** summarize the trade-off between the true positive rate and the positive predictive value for a predictive model using different probability thresholds.
    - ROC curves are appropriate when the observations are balanced between each class, whereas precision-recall curves are appropriate for imbalanced datasets.
- **Bias**: underfitting - algorithm’s error rate on the training set
    - "optimal error rate" ("**unavoidable bias**") - how well can humans do the task roughly?
    - *avoidable* bias: difference between the training error and the optimal error rate.
    - Bias = Optimal error rate (“unavoidable bias”) + Avoidable bias
    - If you have high avoidable bias, **increase the size of your model** (for example, increase the size of your neural network by adding layers/neurons).
        - cons: computational problems because training very large models is slow
        - If you include a well-designed regularization method (e.g. L2, dropout), then you can usually safely increase the size of the model without increasing overfitting
    - You can also:
        - **Modify input features based on insights from error analysis**. In theory, **adding more features could increase the variance**; but if you find this to be the case, then use regularization, which will usually eliminate the increase in variance.
        - **Reduce or eliminate regularization** (L2 regularization, L1 regularization, dropout): This will reduce avoidable bias, but increase variance.
        - **Modify model architecture**
    - Usually not useful to add more training data
- **Variance**: overfitting - how much worse the algorithm does on the dev (or test) set than the training set. Total Error = Bias + Variance
    - In theory, we can always reduce variance to nearly zero by training on *a massive training set*. Thus, all variance is “avoidable” with a sufficiently large dataset, so there is no such thing as “unavoidable variance.”
    - If you have high variance, **add data to your training set**.
        - cons: might also exhaust your ability to acquire more training data
    - **Add regularization** (L2 regularization, L1 regularization, dropout): This technique reduces variance but increases bias.
    - **Add early stopping** (i.e., stop gradient descent early, based on dev set error): This technique reduces variance but increases bias. Early stopping behaves a lot like regularization methods.
    - **Feature selection** to decrease number/type of input features: This technique might help with variance problems, but it might also increase bias. In modern deep learning, when data is plentiful, there has been a shift away from feature selection, and we are now more likely to give all the features we have to the algorithm and let the algorithm sort out which ones to use based on the data. But when your training set is small, feature selection can be very useful.
- **Bias-Variance tradeoff**:
    - high variance, low bias: overfitting
    - high bias, low variance: underfitting
    - high variance, high bias: simultaneously overfitting and underfitting; just bad
    - low variance, low bias: ideal state, great performance
    - Of the changes you could make to most learning algorithms, **there are some that reduce bias errors but at the cost of increasing variance, and vice versa.** This creates a “trade off” between bias and variance.
        - e.g. increasing the size of your model—adding neurons/layers in a neural network, or adding input features—generally reduces bias but could increase variance
        - e.g. adding regularization generally increases bias but reduces variance
        

When you should train and test on *different* distributions (we usually assume they are from the same distribution):

- 10k user-uploaded images, 200k internet sourced random images: instead of putting all 10,000 user-uploaded images into the dev/test sets, we might instead put 5,000 into the dev/test sets. We can put the remaining 5,000 user-uploaded examples into the training set. This way, your training set of 205,000 examples contains some data that comes from your dev/test distribution along with the 200,000 internet images.
- Suppose you are building a speech recognition system to transcribe street addresses for a voice-controlled mobile map/navigation app. You have 20,000 examples of users speaking street addresses. But you also have 500,000 examples of other audio clips with users speaking about other topics. You might take **10,000** (half) examples of street addresses for the dev/test sets, and use the remaining 10,000, plus the additional 500,000 examples, for training.

---

Bagging vs. Boosting:

- **Bagging**: bootstrap aggregation; Bagging is like the basic algorithm for ensembles, except that, instead of fitting the various models to the same data, each new model is fit to a bootstrap resample.
    - **Random Forest**: The random forest is based on applying bagging to decision trees with one important extension: in addition to sampling the records, the algorithm also samples the variables. The random forest method is a “black box” method. It produces more accurate predictions than a simple tree, but the simple tree’s intuitive decision rules are lost. Can know variable importance (either based on accuracy decrease or Gini impurity decrease [how much improvement to the purity of the nodes that variable contributes])
    - Random Forest uses a modification of bagging to build de-correlated trees and then averages the output. As these trees are identically distributed, the bias of Random Forest is the same as that of any individual tree. Therefore we want trees in Random Forest to have low bias. On the other hand, it is fine for these trees to have high variance individually. This is because averaging the trees reduces variance. Combining these two results we end up with deep trees which have low bias and high variance.
- **Boosting**: iterative. fits a series of models with each *successive* model fit to minimize the error of the previous models. weights are increased for the observations that were misclassified; final estimate is calculated weighting different models too, with models with lower error having a bigger weight.
    - XGBoost: XGBoost is an implementation of GBM, with major improvements. GBM’s build trees sequentially, but XGBoost is parallelized. This makes XGBoost faster.
    - Gradient Boosting builds trees in a sequential manner. Roughly speaking, it tries to reduce the loss of the ensemble by fitting a new tree to the negative gradients of the loss function, thus effectively performing gradient descent. Every time the ensemble adds a new tree, its model complexity increases and its overall bias decreases, even if the new tree is quite simple.
        
        On the other hand, while there isn’t much theoretical study on the variance of Gradient Boosting, empirical results show that the variance of the ensemble is not effectively reduced over the boosting process. Therefore for GBDT we use shallow trees with high bias and low variance.
        

**Gradient Descent:**

A cost function measures how close the predicted values are, to the corresponding actual values. Ideally, we want as little difference as possible between the predicted values and the actual values. Thus, we want the cost function to be minimized.

The weights associated with a trained model, cause it to predict values that are close to the actual values. Thus, the better the weights associated with the model, the more accurate are the predicted values and the lower is the cost function. With more records in the training set, the weights are learned and then updated.

Gradient Descent is an iterative optimization algorithm. It is a method to minimize a function having several variables. Thus, Gradient Descent can be used to minimize the cost function. It first runs the model with initial weights, then seeks to minimize the cost function by updating the weights over several iterations.

**Gradient Boosting:**

Boosting: An ensemble of weak learners is built, where the misclassified records are given greater weight (‘boosted’) to correctly predict them in later models. These weak learners are later combined to produce a single strong learner. Gradient Boosting carries the principle of Gradient Descent and Boosting to supervised learning. Gradient Boosted Models (GBM’s) are **trees built sequentially**, in series. In GBM’s, we take the weighted sum of multiple models.

- Each new model uses **Gradient Descent** optimization to update/ make corrections to the weights to be learned by the model to reach a local minima of the cost function.
- The vector of weights assigned to each model is not derived from the misclassifications of the previous model and the resulting increased weights assigned to misclassifications, but is derived from the weights optimized by Gradient Descent to minimize the cost function. The result of Gradient Descent is the same function of the model as the beginning, just with better parameters.
- Gradient **Boosting** adds a new function to the existing function in each step to predict the output. The result of Gradient Boosting is an altogether different function from the beginning, because the result is the addition of multiple functions.

**Regularization**: a technique that modifies the cost function in order to penalize the complexity of the model

- Blind application of xgboost can lead to unstable models as a result of
*overfitting* to the training data. The problem with overfitting is twofold:
    - The accuracy of the model on new data not in the training set will be
    degraded.
    - The predictions from the model are highly variable, leading to unstable
    results.
- for most statistical techniques, **overfitting can be avoided by a judicious selection of predictor variables**.
- **Ridge regression** minimizes the sum of squared residuals plus a penalty on the number and size of the coefficients; doesn’t allow the coefficients to reach zeros. (i.e. **L2**)
- The **Lasso** is similar, except that it uses Manhattan distance instead of Euclidean distance as a penalty term (absolute value instead of square); allows some of the coefficients to reach absolute zero (thereby allowing feature selection). (i.e. **L1**)
- From a practical standpoint, **L1 tends to shrink coefficients to zero** whereas **L2 tends to shrink coefficients evenly**.
    - L1 is therefore useful for feature selection, as we can drop any variables associated with coefficients that go to zero.
    - Namely, in a high dimensional space, you got mostly zeros and a small number of non-zero coefficients. This is huge since it incorporates variable selection to the modeling problem. In addition, if you have to score a large sample with your model, you can have a lot of **computational savings** since you don't have to compute features(predictors) whose coefficient is 0
    - L2, on the other hand, is useful when you have collinear/codependent features. (An example pair of codependent features is gender and ispregnant since, at the current level of medical technology, only females can be ispregnant.) Codependence tends to increase coefficient variance, making coefficients unreliable/unstable, which hurts model generality. L2 reduces the variance of these estimates, which counteracts the effect of co-dependencies.

**Cross-validation:** randomly splits up the data into K different groups, also called folds. For each fold, a model is trained on the data not in the fold and then evaluated on the data in the fold. This yields a measure of accuracy of the model on out-of-sample data. The best set of hyperparameters is the one given by the model with the lowest overall error as computed by averaging the errors from each of the folds.

**Principal Components Analysis** (**PCA**):  a technique to discover the way in which numeric variables covary. The idea in PCA is **to combine multiple numeric predictor variables into a smaller set of variables**, which are weighted linear combinations of the original set. The smaller set of variables, the principal components, “explains” most of the variability of the full set of variables, reducing the dimension of the data. The weights used to form the principal components reveal the relative contributions of the original variables to the new principal components.

- Principal component: A linear combination of the predictor variables.

**K-Means** Clustering:

- **elbow method**, is to identify when the set of clusters explains “most” of the variance in the data. Adding new clusters beyond this set contributes relatively little incremental contribution in the variance explained. The elbow is the point where the cumulative variance explained flattens out after rising steeply, hence the name of the method.
    - a good article:
    
    [Elbow Method for optimal value of k in KMeans - GeeksforGeeks](https://www.geeksforgeeks.org/elbow-method-for-optimal-value-of-k-in-kmeans/)
    

**What is gradient descent? How does it work? ‍⭐️**

- Gradient descent is an algorithm that uses calculus concept of gradient to try and reach local or global minima. It works by taking the negative of the gradient in a point of a given function, and updating that point repeatedly using the calculated negative gradient, until the algorithm reaches a local or global minimum, which will cause future iterations of the algorithm to return values that are equal or too close to the current point.
- 

**What is SGD  —  stochastic gradient descent? What’s the difference with the usual gradient descent? ‍⭐️**

- In both gradient descent (GD) and stochastic gradient descent (SGD), you update a set of parameters in an iterative manner to minimize an error function.
- While in GD, you have to run through ALL the samples in your training set to do a single update for a parameter in a particular iteration, in SGD, on the other hand, you use ONLY ONE or SUBSET of training sample from your training set to do the update for a parameter in a particular iteration. If you use SUBSET, it is called Minibatch Stochastic gradient Descent.

---

 

Start with something simple like Logistic Regression to set a baseline and only make it more complicated if you need to. At that point, tree ensembles, and in particular Random Forests since they are easy to tune, might be the right way to go. If you feel there is still room for improvement, try GBDT or get even fancier and go for Deep Learning.

- Linear Regression:
    - easy to interpret. **the output can be interpreted as a probability: you can use it for ranking instead of classification.**
    - good for cases where features are expected to be roughly linear, and the problem to be linearly separable.
    - can easily "feature engineering" most non-linear features into linear ones
    - suffer multicollinearity
- Decision Tree:
    - Easy to interpret and explain
    - Non-parametric, no need to worry about outliers or whether the data is linearly separable.
    - no distribution requirement
    - heuristic
    - good for few categories variables
    - not suffer multicollinearity (by choosing one of them)
    - can easily overfit
- Gradient Boost Decision Tree:
    - handle very well high dimensional spaces as well as large number of training examples
    - build trees one at a time, each new tree corrects some errors made by the previous trees, the model becomes even more expressive.
    - Harder to tune; longer training time since trees are built sequentially
- Neutral Net:
    - good to model the non-linear data with large number of input features
    - widely used in industry
    - Difficult to debug; not probabilistic

Feature importance: randomForest’s feature importance favors high cardinality (feature with more distinct values); use permutation feature importance instead

**Assumptions of Linear Regression**

- Linear Relationship between features and target variable.
- Multivariate Normality
- No or Little Multicollinearity
    - **Multicollinearity** occurs when more than two independent variables are highly correlated. We can use Variance Inflation Factor (VIF) to measure if VIF > 5 there is highly correlated and if VIF > 10, then there is certainly multicollinearity among the variables.
    - the regression can be unstable or impossible to compute
- No or Little Autocorrelation
- Homoscedasticity (errors don't change much)
    - Constant variance of errors - homoscedasticity. For example, in case of time series, seasonal patterns can increase errors in seasons with higher activity.
- **Additivity** means that the effect of changes in one of the features on the target variable does not depend on values of other features. For example, a model for predicting revenue of a company have of two features - the number of items a sold and the number of items b sold. When company sells more items a the revenue increases and this is independent of the number of items b sold. But, if customers who buy a stop buying b, the additivity assumption is violated.

---

- False negatives—or fraud that is not identified and prevented before a dispute occurs—are not the only way in which fraud can have a real, financial impact on a business.
- False positives—or legitimate transactions that are blocked by a fraud detection system—are also costly: when a customer tries to make a purchase but is prevented from doing so, the business takes both a gross profit and a reputational hit.
- There is a tradeoff between false negatives and false positives—the fewer you have of the former, the more you need to tolerate of the latter (and vice versa). Businesses need to decide how to trade off the two. Each false negative incurs a certain cost (the cost of goods sold and the fee for disputes), as does each false positive (the margin on the goods sold). **If a business’s margins are small, a false negative is very costly and a false positive is not so costly** and so the business should lean towards casting a wide net when trying to stop potential fraud (even if that means more false positives). If margins are large, the reverse is true.
- It’s important to note, however, that businesses are not always truly free to control this tradeoff. If a business’s fraud rate is greater than 1%, card networks like Visa and Mastercard may revoke its ability to process any credit card payments. Once it is above 1%, they must reduce the rate at all costs, even if that means accepting a false positive rate that is higher than is economically rational for the business.

Root mean squared error: the square root of the average squared error of the regression

R-squared: the proportion of variance explained by the model, from 0 to 1. 

- including additional variables always reduces RMSE and increases R-square. Hence, we look at things like AIC (Akaike’s Information Criteria) that penalize adding terms to a model.  (BIC is similar to AIC with a stronger penalty)
- penalized regression is similar in spirit to AIC. Instead of eliminating predictor variables entirely - as with stepwise/forward/backward selection - penalized regression applies the penalty by reducing coefficients, in some cases to near zero. Incl. ridge regression, and lasso regression.

Prediction interval vs. confidence interval: a prediction interval pertains to uncertainty around a single value, while a confidence interval pertains to a mean or other statistic calculated from multiple values. a PI is typically much wider than CI. We essentially select an individual residual to tack on tot he predicted value from a bootstrap model as PI. 

Confounding variables: an important predictor that, when omitted, leads to spurious relationships in a regression equation. 

Outliers: no statistical rule; usually look at “leverage” (measured by hat value), Cook’s distance (leverage + residual size).

Classification: 

- Naive Bayes: uses the probability of observing predictor values, given an outcome, to estimate probability of observing outcome Y = i. Works with categorical predictors and outcomes. It asks, “within each outcome category, which predictor categories are most probable?” That information is then inverted (conditional probability) to estimate probabilities of outcome categories, given predictor values.
- Logistic regression: a special case of a generalized linear model (GLM, with family set to binomial in R); in practice, when choosing cutoff, a lower cutoff is often appropriate if the goal is to identify members of a rare class.
    - no closed-form solution and the model must be fit using maximum likelihood estimation (MLE). MLE is a process that tries to find the model that is most likely to have produced the data we see. It uses a metric called deviance, and a lower deviance corresponds to a better fit.
    - evaluation: accuracy, precision (the % of predicted 1s that are actually 1s), recall (sensitivity).
    - Specificity: the percent of 0s correctly classified
    - ROC curve: a plot of sensitivity vs. specificity
- GLM: has two main components:
    - a probability distribution, or family (binomial in the case of logistic regression)
    - a link function mapping the response to the predictors (logit in the case of logistic regression).
    

Ensemble learning: use many models to form a prediction as opposed to just a single model.