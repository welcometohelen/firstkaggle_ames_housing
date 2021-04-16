# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Project 1: Standardized Tests - still not fun for anyone

### Problem Statement
This technical report consists of two autonomous undertakings: prediction and inference.  Part I answers the question of how to best predict home prices in Ames, Iowa given past data.  Part II answers questions from current homeowners who want to increase the value of their property via remodeling.  Part II is guided by relationships explored in Part I, but the methods used are very different for each in order to preserve interpretability in Part II.

The inferential problem statement as presented is that homeowners in Iowa have a remodel budget and are wondering if they can 'make their money back' via prudent remodeling investments.  They are not shopping for *homes*, so we are working in the confines of fixed neighborhood, street, proximity to amenities, lot size, etc.  This eliminates many of the leading features from the predictive model because prediction was guided by square footage and location.  Are there investments that can improve property value in a meaningful way when these important metrics are already fixed?

---

### Data Dictionary
A thorough [data dictionary](http://jse.amstat.org/v19n3/decock/DataDocumentation.txt) was provided for the Ames housing data - thank heavens

---

### Methods
##### Prediction
Cleaning and EDA were heavily guided by sources from Kaggle, in-class, and Medium.  The popularity of this exercise meant there was a wealth of information out there, but it also got messy to reconcile or combine different methods.  Initial cleaning took place in a separate notebook with only the training data.  Once my methods were established, I created this submission notebook where I first merge the train and test data in order to clean them simultaneously.  This helped avoid shape errors from dummies, etc, but since I was working off an existing template data leakage was low risk.  

Cleaning involved filling in NAs with values as justified in the data dictionary, dropping 2 outliers as suggested in the data dictionary, binarizing when possible, and replacing a *small* handful of remaining nulls with the median of that feature.  After that, the main transformations in my best scoring model were:
* log transforming the target (saleprice) via logp1
* squaring the ordinal scales so higher ranks were more heavily rewarded
* identifying skew>0.5 among all *continuous* variables, and logp1 scaling them as well
* letting Lasso do it's thing wrangling many features instead of trying to select for it

##### Inference
Using the pre-transformed cleaned data from Part I, I proceeded to clean it all again entirely.  With the goal of wanting to present the audience with few, succinct features to prioritize, I rebinned many of the categorial and ordinal variables.  I did not tinker with skew (aside from logging the target), and I leaned heavily on heatmaps as they provide an intuitive glance at many variables.  I was very selective here, culling out down to seven 'renovatable' features and performing linear regression, instead of turning to regularized regression.

---
### Summary
##### Prediction
Lasso was the superior regression tool for how I cleaned my data.  Linear Regression was actually a close second for a while, which surprised me given the boost in complexity that Lasso imparts.  Through all the predictive models, Test RMSE was very close to Train RMSE, so I was not concerned about high variance.  Which was annoying because removing variables is easy.  Setting the scoring parameter to 'neg_mean_squared_error' improved the Ridge RMSE even with all else held equal.  However, it was still barely surpassed by Lasso.

##### Inference
Interpretability proved a bear.  When including many features, coefficients did not always correspond with common sense.  It took more pruning and rebinning of features to yield information that could be useful to current homeowners.  Overall, the most influential (and mutable) attributes of a home were:
* Kitchen quality
* Number of bathrooms
* Paved driveway
* Central Air
Each of these features imparted over 16% increase in saleprice per 1 unit change, ceteris paribus.  Other attributes are still useful to the stakeholder even though less impactful.  Basement finish and Heating, for example, may yield only a 2.5-3% return, but they can be very cheap investments depending on the current state of each feature.

---
### Conclusions
This exercise absolutely illustrated the difference between inference and prediction.  Prediction led to all kinds of transformations and throwing maybe-sticky-things at the wall, and if I had tried to maintain that methodology in the Inferential section, it would have been untenable.  Simple linear regression was how I kept information interpretable for stakeholders, but the specificity and value of my results suffered because of it.  Future goals are to revisit this Kaggle exercise and/or similar competitions because honestly I feel like I lucked into decent scores as opposed to heading there with purpose.  For inference, this report is supersaturated with heatmaps and I need to practice other ways to convey information to the less data-savvy.