---
layout: post
title: An Introductory Look at Multi-Class Classification
---

### Introduction
For elite basketball players, the ultimate goal is to make it to the NBA. While the number of players, especially underclassmen, declaring for the draft is increasing every year, only 60 players will be lucky to start their professional careers through the NBA draft. The decision to declare for the draft is not always simple. There can be significant loss for those that go undrafted in the form of lost college eligibility. There is also a significant financial difference between going in the 1st round vs 2nd round. Therefore for these players, it is very important to know where they can expect to be drafted before making a decision.

### Goal
Create a classification model to predict where a college basketball player will go in the NBA draft.

### The Data
Data was scraped using this [API](https://sportsreference.readthedocs.io/en/stable/) for [Sports-Reference.com](http://www.sports-reference.com) which includes yearly and career stats for all Division I college players. To filter this down, I created a list of players who had reasonable chances of being drafted. This prospects list was aggregated from several sources including previous NBA draft results, NBA pre-draft combine invites, lists of underclassmen who officially declared for the draft, and draft big board rankings from [NBADraft.net](https://www.nbadraft.net/ranking/bigboard/). The final dataset included players entering the draft from the years 2011 to 2019. Statistics from each player's most recent year in college was used in the analysis. 

### Classification Modeling
The problem was approached as a multi-class problem with 3 draft outcomes - round 1, round 2 , and undrafted. We will label these as Class 1, Class 2, and Class 3 respectively. Looking at the outcome measure, there was a slight imbalance in the dataset but not felt to be large enough to warrant resampling. 

![Class Balance]({{ site.url }}/images/2020-05-12/class_balance.png)

Three classification models were used for analysis including k-nearest neighbors, logistic regression, and random forest.

### Error Metrics in Multi-Class
Before diving deeper into the models, it was important to decide what metrics I would be evaluating them on. In binary classification problems, standard metrics may include accuracy, precision, recall, and f1. For multi-class, we can calculate these same metrics by converting the data into a series of binary problems, ie One-vs-all. This will give us 3 values for each metric, one for each outcome class. Additionally, we can calculate an overall score by implementing a weighting scheme. There are 2 primary weighting options - micro vs macro. In micro weighting, each data point is given equal weight which will naturally give more weight to majority classes. In macro weighting, metrics are calculated for each class individually and then averaged together. This can give more weight to minority classes relative to micro weighting.

*Note: In micro-weighted scores, Accuracy = Precision = Recall = F1. 

Which score is best depends on what goals we have. Thinking back to the case problem, I decided on the following priorites:
#### 1. Maximize recall of going undrafted
The worst case scenario is a player expecting to be drafted (potentially giving up college eligibility) and then failing to do so. To minimize this I want to reduce the number of false negatives when predicting going undrafted. This would be reflected in higher recall scores for class 3. 

#### 2. Maximize both precision and recall of predicting going in the 1st round
In predicting which players will go in the 1st round, I care about precision and recall equally. Similar to above, I want to minimize players potentially giving up college eligibility expecting to go in the first round, only to drop to the 2nd round or lower. At the same time, there is so much to be gained from being drafted in the 1st round, that I want to capture as much of those cases as possible. F1 score of class 1 would give a good balance of both recall and precision.

From these 2 priorities, it's implied that predicting class 3 carries the most weight, followed by class 1, and then class 2. This happens to align with the natural imbalance in the dataset. Therefore, this can be used to our advantage with micro-weighted averages when looking at an overall accuracy score.

In summary, I will use these metrics to evaluate the classification models:
1. Class 0 Recall
2. Class 1 F1
3. Micro-weighted Accuracy

### Selecting a Model
The data was split into training and test sets. After running the 3 classification models on the training data with 10-fold cross validation, logistic regression and random forest appeared to perform best. The error metrics for both models are as follows:

Logistic Regression metrics:
![Logistic Regression scores]({{ site.url }}/images/2020-05-12/lr_val_table.png)

Random Forest metrics:
![Random Forest scores]({{ site.url }}/images/2020-05-12/rf_val_table.png)

Although the random forest model has slightly higher overall accuracy, it performs lower in the first two metrics of interest - class 0 recall and class 1 F1. Therefore, I decided to use logistic regression as my final model. After retraining on the entire training set, here is how it performs on the set-aside test data:

![Logistic Regression Confusion Matrix]({{ site.url }}/images/2020-05-12/lr_test_matrix.png)

The final result is a classification model that prioritizes class 1 and class 3. In fact, it weights these 2 classes to the point that the model resembles binary classification with almost no predictions made for class 2. With more time, it would be interesting to see how the models would look with additional fine-tuning of class weights.

### Conclusion
In order to properly evaluate a model, it is necessary to determine what metrics will be used for evaluation. This is especially true for multi-class problems which can be more difficult to summarize with a single measure. 