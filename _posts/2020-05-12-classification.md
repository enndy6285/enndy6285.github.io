---
layout: post
title: Introduction to Multi-Class Classification
---

### Introduction
For elite basketball players, the ultimate goal is to make it to the NBA.





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

### Selecting a Model



### Conclusion
