# Description of my solution to the "Predict Future Sales" project

## Model summary

### Summary

I use an ensembling approach, where first-level models are

* linear Ridge regression
* k-nearest neighbour
* gradient boosting
* neural network.

The most important features are

1. number of items sold in each shop over the previous month
1. number of month since an item was first sold
1. aggregate volume of sold item over the previous month
1. number of items sold in each shop over one month, two months before
1. same as above, three months before

On my machine (4 CPU and 8Gb of RAM) the whole ensembling model takes
about six hours to train, the most computationally intensive bit being
the kNN model fit.

### Feature selection and engineering

The last two train months correspond to validation sets for the first-level
models trained using all the train data _before_ each validation set.

The respective importance of the features is an indicator of which variables to
keep and which to remove. I did this manually, and also using common sense
as to which variables could be most effective.

Different shop ID's refer to the same shops, according to their names, so I
assign a common ID to those pairs of shops.

Information is extracted from shop names, namely city names, and from category
names, namely meta-categories and sub-categories.

A very informative variable is the time elapsed since the first same item was
sold.

Various aggregates across shop ID's, item ID's, (meta-/sub-)category ID's,
cities were tried, as well as the standard deviation thereof.

### Training methods

I penalise the linear regression model using the Ridge scheme, which is faster
to train than Lasso and yields similar results. This is the fastest of the four
first-level models to train, and yields the third-best results.

The kNN model suffers from the curse of dimensionality, so I first reduce
the dimension of the problem using PCA, training kNN on the first six components
representing more than 98% of the variance of the original training set.
This approach is the slowest of the four and seems to perform relatively poorly.

For gradient boosting I use catboost and early stopping on the validation sets
to identify the best number of trees. We investigate different learning rates
and L2-penalisation factors that we test against the validation sets.

The neural network has a simple architecture with two fully-connected hidden
layers. It does not require much tweaking and converges in four epochs.

At the top level, we try a convex combination of the four model predictions,
a linear regression, and a shallow gradient boosting model. These models are
trained on meta-features (predictions from first-level model) on ten months
and validated on the last month (block 33).

### Interesting findings

Although the yearly seasonality is clear from the exploratory analysis, it
appears that features lagged by 12 months are mostly insignificant to the
models.

I also tried to fit the same models without features lagged by 12 months,
which also allows for reducing the amount of data removed during
pre-processing (especially November 2013, so we have two November months
to train on). However the results were not improved.


