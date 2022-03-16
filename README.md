# Flight Delay Analysis

## Introduction

Primary objective of this project is to understand how factors such as region/city destination and length of travel affects expected flight delay time. 

## Dataset

The `united_summer2015.csv` dataset contains United Airlines domestic flights departing from San Francisco in the summer of 2015 (data collected from the Bureau of Transportation Statistics in the United States Department of Transportation).

## Airport Data Analysis

**# what is the distribution of number of flights per region?**

[image]

```R
1 Midwest   1806
2 Northeast 2897
3 South     3074
4 West      5277
```

Based on the Histogram the most delay time is in the West region, and the lowest delay time is in the Midwest region.

**# maximum delays per month?**

```R
 Delay City        month Date  state
1  438 Los Angeles July  7/23/15 CA  
2  537 Honolulu    June  6/22/15 HI  
3  458 Chicago     August 8/23/15 IL
```

For each month, the Honolulu, HI has the most delay time on June. The Los Angeles, CA has longest delay time on July. And, the Chicago, IL has longest delay on August.

**# Is there any statistically differences between the mean time delay of 37 cities across the country? (Using ANOVA)**

Under a 95% confidence interval with alpha = .05, the hypothesis tests are:

```
Ho: mean(ANC_delays) == mean(PHX_delays) == mean(LAX_delays) == mean(SAN_delays) == … == mean(IAD_delays) == mean(MSN_delays) (This is the mean delay time of each airport arriving from San Francisco).

Ha: mean(ANC_delays) != mean(PHX_delays) != mean(LAX_delays) != mean(SAN_delays) != … != mean(IAD_delays) != mean(MSN_delays)
```

Whether the mean average delays across all cities are roughly equivalent (Ho) or there exists at least one city with significantly different average delays (Ha). Below is the result of the ANOVA test:

```
Analysis of Variance Table
Response: delays
       Df  Sum Sq Mean Sq F value   Pr(>F)
 cities    36  130874 3635.4  2.341 1.008e-05 ***
 Residuals 13017 20214217 1552.9           
 Residual standard error: 39.41 on 13017 degrees of freedom
 Multiple R-squared: 0.006433,  Adjusted R-squared: 0.003685 
 F-statistic: 2.341 on 36 and 13017 DF, p-value: 1.008e-05
```

**Interpretation:** After conducting the ANOVA test, we find that the p-value for mean delay time of each airports is approximately zero, which is less than 0.05 as given alpha value above. We are confident to reject the null hypothesis stating that mean delay time from 37 airports are the same. 

However, does our data meet ANOVA's normality assumption to make the results reliable?

[IMAGE]

From the above images, we notice that the qqplot and histogram of delay time from these airports are not normally distributed. There are some airports containing low amount of data points. For example, MSY and IAD airports only have 3 and 2 data points respectively. For this reason, we propose to conduct permutation test to determine whether or not the mean delay time is affected by the specific city a flight is travelling to. The p-value of the permutation test is demonstrated below:

```R
[1] p-value is 0.002 
```

**Decision:** The p-value is 0.002 which is less than alpha value of 0.05. Thus, we reject the null hypothesis that average delays are not significantly different between cities. 

**# what is the average delay times of each region?**

To gauge for the true average delay time per region, we conduct a bootstrap test with a 95% confidence interval:

```
The 95% CI Delay time for the West is between 14 and 16 minutes.
The 95% CI Delay time for the South is between 14 and 17 minutes.
The 95% CI Delay time for the North is between 16 and 19 minutes.
The 95% CI delay time for the Midwest is between 17 and 20 minutes.
```

We can see that the Midwest has the highest delays ranging from 17 to 21 minutes on average. Conversely, we see that the West region has the lowest amount of delays with an interval between 14 and 16 minutes. 

**# which days of the week have the most delays?** 

Again, to gauge the true average delay per day of the week, we conduct a boostratp test with a 95% confidence interval:

```
The 95% CI delay given for Monday is between 14 and 19 minutes.
The 95% CI Delay given for Tuesday is between 9 and 14 minutes.
The 95% CI Delay given for Wednesday is between 24 and 31 minutes.
The 95% CI Delay given for Thursday is between 9 and 14 minutes.
The 95% CI delay given for Friday is between 12 and 17 minutes.
The 95% CI delay given for Saturday is between 15 and 20minutes.
The 95% CI delay given for Sunday is between 11 and 17 minutes.
```

We can see from our confidence interval that the best day to travel is on Tuesday and with a similar lower delay time being on Thursday. Thus, we can say that we are 95% percent confident that Tuesday delay time will fall between 9.82 and 14.33 minutes. On the other hand, the day with the most delay is Wednesday with the average delay being between 24 .37 and 31.66 minutes. It’s safe to say that the best days to travel are on Tuesday and Thursday. On the contrary we can say that the worst days to travel are Wednesday and Saturday.

**# is there a difference in average delay times between distances?**

To calculate whether average delay times differ significantly between distances, we must first categorize each flight into three distance categories: flights that travel (0-1], (1-2], (2-3] thousand files. Then, we conduct below ANOVA test:

```
[1] "Conducting ANOVA test on our data:"
[1] "We will conduct Levene's test for variance homogeneity to assert whether our data passes such constraint required by ANOVA."
[1] "Ho: Variance is homogenous"
[1] "Ha: Variance is NOT homogenous"
Given that our p-value is 0.0913544 , which is > 0.05 ,data passed homogeneity test of variance, thus we accept Ho. 
[1] "Permuted ANOVA test:"
[1] "Ho: Differences in average delay times are not significant between distances"
[1] "Ha: Differences in average delay times are significant between distances "
Given that our p-value is 0.0001353881, which is < 0.05 , we reject the notion that average Delay between distances are not statistically significant.
Thus, we reject Ho and now perform pairwise testing to explore which group(s) are statistically significantly different from each other.
[1] "Pairwise Testing:"
[1] "Given that normality was NOT met, we will perform Wilcox's pairwise non-parametric test"
```

[IMAGE]

From the above graph, we see **all pairwise comparisons with flights from 0 to 1000 miles are significantly different. Therefore, we can expect average delay time from 0 to 1000 miles to be significantly different against distances from 1000 to 2000 miles or 2000 to 3000 miles.** 
 

From the above, we have found two main ideas under an alpha level of 0.05 :

1. Average Delay are significantly different between distances 
2. Discovered pairwise distances with significant average delay.

## Conclusions

From the analysis there a few key things that come to mind and that we must acknowledge. The average delay time for each airport is not the same. Also, the average delay for each city is different. Further, we see that the average delay time for the first 1000 miles is significantly different than traveling for an additional 1000 miles or two thousand miles. Another important thing to remember thing to note is that the best time to travel with lowest amount of delays is Tuesday and Thursday. Finally, we can say that the region with the highest interval of delays would be the Midwest with a minute more on average in delay.  