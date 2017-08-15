**Title**: Wide & Deep Learning for Recommender Systems

**Author**: Google Inc. 

**Publish time**: 2016

**Application scenario**:
For one recommending request, the retrieval system first give top 10 candidates app, then the goal of this paper is to rank the candidates based on the final prediction score for each app.


Wide component (i.e., memorization)

Features:
raw features and cross-product transformation features (co-occurrence of raw features)

Score function:
generalized linear model
where the maximum of `k` is 2 of the `d`th power

Optimization:
Follow-the-regularized-leader (FTRL) with L1 regularization


Deep component (i.e., generalization)

Features:
concatenated embeddings of categorical feature

Score function:
three hidden layers with ReLU activation

Optimization:
AdaGrad

Combined loss function:
(3)

Preprocessing
Continuous real-valued features are normalized to [0, 1] by mapping a feature value x to its cumulative distribution function
P(X <= x), divided into nq quantiles.
feature engineering

training data
The label is app acquisition:
1 if the impressed app was installed, and 0 otherwise.



Memorization can be loosely defined as learning the frequent co-occurrence of items or features and exploiting the correlation available in the historical data.
