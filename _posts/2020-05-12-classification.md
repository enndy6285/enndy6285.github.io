---
layout: post
title: Introduction to Multi-Class Classification
---

### Introduction
For elite basketball players, the ultimate goal is to make it to the NBA.





### The Data
Data was scraped using this [API](https://sportsreference.readthedocs.io/en/stable/) for [Sports-Reference.com](http://www.sports-reference.com).  using BeautifulSoup. The final dataset includes movies released between the years of 1980 and 2019 that had a wide release (screened in at least 600 theatres).

![Class Balance]({{ site.url }}/images/2020-05-12/class_balance.png)

### Classification Modeling

The problem was approached as a multi-class problem with 3 draft outcomes - round 1, round 2 , and undrafted. Three classification models were used to analy

#### Error Metrics in Multi-Class
For a linear regression model to be successful, it must satisfy the assumptions of linear regression. As I learned throughout this project, this is very challenging in practice.


#### Selecting a Model


# Assumption #1 - Linearity
Ideally there is a linear relationship between the dependent and indepdent variables if we are trying to fit a linear regression model... 

Unfortunately this poses some immediate challenges since we are dealing with price. Since box office earnings cannot not go below zero on the lower bound and can be infinitely high on the upper bound, this can cause linearity to break down. Looking at an initial plot of Actual vs Predicted values, we can see linear regression is not performing as well as we would hope.

![Actual vs. Predicted]({{ site.url }}/images/2020-04-21/actual_pred.png)

One potential solution is to transform the data. In this case, all price related variables were log-transformed which improved the linear relationship.

![Actual vs. Predicted (log)]({{ site.url }}/images/2020-04-21/log_act_pred.png)

Another challenge to assuming linearity between dependent and independent variables is the presence of a confounding variable. Through initial EDA, it was apparent that trends over time existed for several independent variables (budget, number of theatres) AND the dependent variable.

![Pairplot]({{ site.url }}/images/2020-04-21/pairplot1.png)

To account for this, all price related variables were adjusted for average yearly ticket price as a measure of price inflation over time.


#### Assumption #2 - Homoscedasticity
Error terms should have constant variance. My initial model showed a very high degree of heterscedasticity with increasing variance at higher predicted values. Log transformation again helped in this case although we can see the variances are still not completely uniform.

![Residual Plot]({{ site.url }}/images/2020-04-21/resid.png)
![Residual Plot (log)]({{ site.url }}/images/2020-04-21/log_resid.png)


#### Assumption #3 - No Multi-Collinearity
The independent variables should not have any linear relationships with each other. One of the movie features (Number of Theatres released) initially present in the model was eventually removed due to strong colinear effects with budget. This makes intuitive sense as a studio would want to release a movie that cost a lot of money to make in as many theatres as possible in order to maximize return on investment. This variable may have also had a dependent relationship on our dependent outcome since more theatres are likely to screen a movie that is already performing well.

A degree of multi-collinearity was also introduced with the cast/crew independent variables. For these features, numerical values were calculated based on the box office earnings of previous movies that the cast/crew member in question had been involved in. This has some overlap with movie budget as more established actors/directors/writers would likely demand higher salaries.


#### Assumption #4 - Independence of Errors
This generally holds true since each observation is a different movie. Even though there were some time trends seen, the data does not represent a time series. This assumption does break down, however, when thinking about the effect of movie franchises which was not accounted for in the model. Some inter-dependence between observations was also introduced with the numerical cast/crew values created above.


#### Assumption #5 - Normality of Errors
Errors should follow a normal distribution. We can see this was followed fairly well in the error distribution plot below.

![Error Distribution]({{ site.url }}/images/2020-04-21/resid_hist.png)


### The Final Model
After feature transformation and engineering, the data was modeled with cross-validation with simple OLS, Ridge, and Lasso regression. R-squared values were similar between all 3 models around 0.43. Insights from the model include:
1. Budget had the largest impact with higher budgets predicting higher domestic gross
2. The best months to release a movie are May, June, and July
3. Director and writer had slightly more impact compared to actors on box office gross
4. R-ratings predicted lower box office numbers


### Conclusion
In trying to model the domestic movie box office with linear regression, I learned that real world data rarely follows all of the assumptions of linear regression perfectly. I was not too surprised to see my final model have a very modest R-squared score of 0.43. Nevertheless, this project highlighted the importance of keeping the assumptions in mind not just during initial model selection but through the entire analytical process including feature selection and engineering.