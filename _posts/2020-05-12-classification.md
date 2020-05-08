---
layout: post
title: Introduction to Multi-Class Classification
---

### Introduction
For elite basketball players, the ultimate goal is to make it to the NBA.





### The Data
Data was scraped using this [API](https://sportsreference.readthedocs.io/en/stable/) for [Sports-Reference.com](http://www.sports-reference.com) which includes yearly and career stats for all Division I college players. To filter this down, I created a list of players who had reasonable chances of being drafted. This list was aggregated from several sources including previous NBA draft results, NBA pre-draft combine invites, lists of underclassmen who officially declared for the draft, and draft big board rankings from [NBADraft.net](https://www.nbadraft.net/ranking/bigboard/). The final dataset included players entering the draft from the years 2011 to 2019. Statistics from each player's most recent year in college was used in the analysis. 

### Classification Modeling
The problem was approached as a multi-class problem with 3 draft outcomes - round 1, round 2 , and undrafted. Looking at the outcome measure, there was a slight imbalance in the dataset but not felt to be large enough to warrant resampling. 

![Class Balance]({{ site.url }}/images/2020-05-12/class_balance.png)

Three classification models were used for analysis including k-nearest neighbors, logistic regression, and random forest.

### Error Metrics in Multi-Class
Before diving deeper into the models, it was important to decide what metrics I would be evaluating them on. In binary classification problems, standard metrics may include accuracy, precision, and recall. This 


### Selecting a Model



### Conclusion
In trying to model the domestic movie box office with linear regression, I learned that real world data rarely follows all of the assumptions of linear regression perfectly. I was not too surprised to see my final model have a very modest R-squared score of 0.43. Nevertheless, this project highlighted the importance of keeping the assumptions in mind not just during initial model selection but through the entire analytical process including feature selection and engineering.