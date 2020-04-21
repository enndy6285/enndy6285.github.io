---
layout: post
title: Modeling Movie Box Office with Linear Regression
---

For this project, I used data scraped on movies to explore modeling with linear regression.

### Goal
As before, the first thing I want to establish is a clear question that I am trying to answer with the data. I decided I wanted to put myself in the seat of a movie studio producer to see if I can make the highest grossing movie. With this in mind, for independent variables I focused on movie features that would be present leading up to its release date. The dependent outcome was set as total domestic box office for a movie. 


### The Data
Data was scraped from [BoxOfficeMojo](http://www.boxofficemojo.com) and [IMDB](http://www.imdb.com) using BeautifulSoup. The final dataset includes movies released between the years of 1980 and 2019 that had a wide release (screened in at least 600 theatres).


### Linear Regression Modeling
For a linear regression model to be successful, it must satisfy the assumptions of linear regression. As I learned throughout this project, this is very challenging in practice.

#### Assumption #1 - Linearity
Ideally there is a linear relationship between the dependent and indepdent variables if we are trying to fit a linear regression model... 

Unfortunately this poses some immediate challenges since we are dealing with price. Since box office earnings cannot not go below zero on the lower bound and can be infinitely high on the upper bound, this can cause linearity to break down. Looking at an initial plot of Actual vs Predicted values, we can see linear regression is not performing as well as we would hope.

![Actual vs. Predicted]({{ site.url }}/images/2020-04-21/actual_pred.png)


### Insights
We first aggregated the data for the entire month to get total entries through each station. We can then sort to find the top overall stations by total foot traffic.


![Total Station Entries]({{ site.url }}/images/station_total.png)


Next we wanted to see how this varied over time. Breaking the data down by day of the week, it is clear traffic is higher on weekdays and drops significantly on the weekday. This pattern is consistent among all of our top stations.


![Station Entries by Day of the Week]({{ site.url }}/images/station_DOF.png)


Recalculating the total station entries during weekday onlys, the top stations remains almost the same, with Fulton St and 42 St-Port Authority swapping the 4th and 5th positions.


![Total Station Entries on Weekdays]({{ site.url }}/images/station_total_weekdays.png)


Each day can then further be broken by time of the day. We can now see that highest traffic occurs during the evening hours of 4-8 pm. Intuitively, this checks out as this would align with the return work commute.


![Top 7 Stations Heatmap]({{ site.url }}/images/station_top7_heatmap.png)



### Conclusions and Future Work
Based on these results, we would recommend WTWY focus their resources at the following stations, on weekdays between 4 and 8pm.
1. Grand Central and 42nd Street
2. 34th Street and Herald Square
3. 14th Street and Union Square
4. Fulton Street
5. 42nd Street and Port Authority

Thinking more for our client, there is one aim that we haven't yet honed in on which would be great to explore in the future. Namely, how can we target NYC subway riders who are most likely to attend the gala? This would include looking into characteristics of previous gala attendees. We would also want to incorporate proximity data to see how these top stations relate geographically to major tech companies in NYC. Also, would there be value in sending team members to local universities to recruit students who are interested in the cause of women in tech? Just a few things we can explore next.