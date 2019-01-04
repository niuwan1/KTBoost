# KTBoost - A Python Package for Boosting

This Python package implements several boosting algorithms with different combinations of base learners, optimization algorithms, and loss functions.

### Description

Concerning **base learners**, KTboost includes:

* Trees 
* Kernel Ridge regression
* A combination of the two (i.e., the KTBoost algorithm) 


Concerning the **optimization** step for finding the boosting updates, the package supports:

* Gradient descent
* Newton-Rahson method
* A hybrid version of the two for trees as base learners


The package implements the following **loss functions**:

* **Continuous data** ("regression"): quadratic loss (L2 loss), absolute error (L1 loss), Huber loss, quantile regression loss, Gamma regression loss, negative Gaussian likelihood with both the mean and the standard deviation as functions of features
* **Count data** ("regression"): Poisson regression loss
* (Unorderd) **Categorical data** ("classification"): logistic regression loss (log loss), exponential loss, cross entropy loss with softmax
* **Mixed continuous-categorical data** ("censored regression"): negative Tobit likelihood (i.e., the Grabit model)




### Installation

It can be **installed** using 
```
pip install -U KTBoost
```
and then loaded using 
```
import KTBoost.KTBoost as KTBoost
```

### Usage and examples
The package re-uses code from scikit-learn and its workflow is very similar to that of scikit-learn.

The two main classes are `KTBoost.BoostingClassifier` and `KTBoost.BoostingRegressor`. 

The following **code example** defines models, trains them, and makes predictions.

```python
import KTBoost.KTBoost as KTBoost

######################################
## Define models (several examples) ##
######################################
## Standard tree boosting for regression with quadratic loss and hybrid gradient-Newton updates as in Friedman (2001)
model = KTBoost.BoostingRegressor(loss='ls')
## Grabit model as in Sigrist and Hirnschall (2018)
model = KTBoost.BoostingRegressor(loss='tobit')
## KTBoost algorithm for classification with Newton updates
model = KTBoost.BoostingClassifier(loss='deviance',base_learner='combined',update_step='newton')
## Gradient boosting for classification with trees as base learners
model = KTBoost.BoostingClassifier(loss='deviance',update_step='gradient')
## Newton boosting for classification model with trees as base learners
model = KTBoost.BoostingClassifier(loss='deviance',update_step='newton')
## Hybrid gradient-Newton boosting (Friedman, 2001) for classification with trees as base learners
model = KTBoost.BoostingClassifier(loss='deviance',update_step='hybrid')
## Kernel boosting for regression with quadratic loss
model = KTBoost.BoostingRegressor(loss='ls',base_learner='kernel')
## Regression model where both the mean and the standard deviation depend on the covariates / features
model = KTBoost.BoostingRegressor(loss='msr')

##################
## Train models ##
##################
model.fit(Xtrain,ytrain)

######################
## Make predictions ##
######################
model.predict(Xpred)

```

### References

* Friedman, J., Hastie, T., & Tibshirani, R. (2000). Additive logistic regression: a statistical view of boosting. The annals of statistics, 28(2), 337-407.
* Friedman, J. H. (2001). Greedy function approximation: a gradient boosting machine. Annals of statistics, 1189-1232.
* Sigrist, F. (2018). Gradient and Newton Boosting for Classification and Regression. arXiv preprint arXiv:1808.03064.
* Sigrist, F., & Hirnschall, C. (2017). Grabit: Gradient Tree Boosted Tobit Models for Default Prediction. arXiv preprint arXiv:1711.08695.