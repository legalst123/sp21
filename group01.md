---
layout: page
title: Group 01
nav_order: 6
description:
---
# Predicting Gun Violence by State

## Introduction

---

### **Question: What influences gun violence in different states?**

Our project focused on studying the various factors contributing to gun violence occurring in the United States. We analyzed trends of gun violence in different states and observe factors like state laws, public sentiment towards gun control, gun purchase data, and more to develop an empirical understanding of what influences gun violence.

Our initial data was drawn from the Gun Violence Archive$^1$, which includes a collection of U.S. gun violence incidents from over 2,000 sources from the years 2014 to 2016. This includes a multitude of datasets on mass shootings, accidental killings by age, and more all grouped by state and county. We began by adding all of these features into one data frame and normalizing them by population. 

In this write-up, we will analyze our methodology and findings regarding gun violence influence.

## Motivation

---

Gun violence has increasingly become a problem in the United States. In the United States, 73% of homicides are committed using guns, compared to the 39% in Canada, and the 4% in the U.K.²  Gun-related deaths in America are increasingly a problem, especially compared to other leading global countries.

While gun violence remains high in the U.S., attitudes towards guns remain contrary. In 2020, approximately 60% of respondents desired stricter gun laws, and less than 10% of respondents wanted less strict gun laws. This is a testament to the growing public awareness of gun violence, and the public's desire to see gun reform.² 

Ultimately, these two points, the increase in gun violence in the U.S., and growing anti-gun sentiment in America sprouted our interest in studying the relationship between the two. In this report, we will utilize public sentiment and other factors various factors to analyze their respective influences on gun violence. 

## Variable Classification

---

Before diving into our data analysis, we have defined the variables we used below.  Our data is indexed by state and year, so each of the following variables is aggregated by these two indices. 

[Variable](https://www.notion.so/856be88f1b244e728192b12815cd040d)

In addition to these variables, we had several more variables that were used to understand the breakdown of gun law categories by state. The following categories were column names in our dataframe, and their values contained the number of gun laws pertaining to that category.

So, for example, if Alaska had 6 laws about background checks, then the value at that cell would be 6. The law categories were found from the RAND State Firearm Law Database$^4$, and were broken up into the following categories: 

1. background checks 
2. carrying a concealed weapon (ccw)
3. castle doctrine 
4. child access laws
5. dealer license 
6. firearm removal at scene of domestic violence 
7. firearm sales restrictions 
8. firearms in college/university 
9. local laws preempted by state
10. minimum age 
11. open carry 
12. permit to purchase 
13. prohibited possessor 
14. registration 
15. required reporting of lost or stolen firearms 
16. safety training required 
17. waiting period  

Counting the number of laws per state in each of these categories is not the best proxy for how restrictive the laws themselves are. However, this was the simplest way for us to try to quantify law strictness in each of these categories. Additionally, by counting the number of laws per state, we are able to create a numerical variable that will allow us to create data visualizations and models.

## Gun Archive Exploratory Data Analysis

---

As a first step, to understand the trends and patterns in our data, we began our analysis with simple visualizations. This included comparing certain features across different states to better understand the differences between states as well as plotting correlations to visually demonstrate the importance of various features in predicting the gun death rate. 

The first visualization we crafted was a pie chart showing the percentage breakdown of shootings by state. The highest rates of shootings are in California (11.99%) and Illinois (9.15%), then Florida (7.36%) and Texas (6.41%). This shows that these states accounted for a significant chunk of all shootings in America. 

Empirically, this makes sense, 3 of these 4 states are the highest populated states in the country. That being said, it is valuable to adjust the rates of shootings by population, as it is likely that states with higher populations will experience more gun violence.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled.png](Untitled.png)

Adjusting the shooting rates by a state's population paints a different picture. As shown in the bar chart below, the rate of shootings per capita is starkly different from pure number of shootings.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%201.png](Untitled%201.png)

After adjusting for population, the story becomes different. Illinois remains in the top 3 for shootings per capita per state. D.C. and Louisiana come in next, claiming the 2nd and 3rd highest number of shootings per capita.

The states with the most shootings, California, Florida, and Texas, sink to around the average or lower when accounting for population, indicating **population is the central factor i**n the explanation for the number of shootings that occur in a state, but not the only factor.

One empirical hypothesis for why D.C. leads in proportional shootings is that D.C. is a very dense metropolitan area. Unlike states that have a mix of city and rural areas, Washington D.C. is the American capital, and mostly only contains densely populated city spaces serving government and metropolitan needs. Further, the population of Washington D.C. fluctuates extensively, as the population mostly consists of traveling politicians and leaders.

We then looked into injuries by the state to understand how this contrasts with the percentage breakdown of shootings per capita. **The following bar graph shows a striking similarity to the pie chart above**. California, Florida, and Illinois are the leaders in total injured across states, and these states were also the leaders in shootings per state. Once again, the population-adjusted analysis would be a useful next step here.

Logically, this correlation makes sense, as the number of shootings should correlate closely to the number of injuries in a state. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%202.png](Untitled%202.png)

Finally, in order to account for population, we created a map graph detailing gun death rate by state. Below, this map helps us geographically visualize the gun death rates across states. 

Using statistics from Gifford's Law Center Gun to Prevent Gun Violence$^2$, we ranked states by their 2020 gun death rate. States in the South-Eastern part of the United States dominate the top ten rates, including Mississippi, Missouri, Alabama, and Louisiana, corroborating the above results. 

This is a far more accurate measure of shootings and their impacts on a state-by-state basis. This implies that some states like Illinois may have a high proportion of shootings, but they are on average more likely to be non-fatal than other states like Missouri.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/gun_death_rate.png](gun_death_rate.png)

After gaining this initial understanding of how shootings and injuries are distributed and ranked across states, we then shifted our approach to understanding different factors contributing to these metrics. Our next step was to dive into gun purchases by state and understand how gun purchases, background checks, and acquisition influenced gun violence.

## Background Checks and Permits Data

---

We used data to understand if gun violence has an existing correlation with the number of guns being purchased, permits issued, and the number of background checks conducted. 

Raw gun purchase data is inaccessible since many guns being sold by private businesses. As a result, the standard for tracking gun purchases is by using background check data from the FBI's National Instant Criminal Background Check System (NICS) to track purchases via background checks performed on each sale$^3$. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%203.png](Untitled%203.png)

The first visualization we created was for the unadjusted number of permits issued by a state, for which Kentucky, California, and Illinois have the most.

While this data is powerful and provides us some really great insight into permits and background checks, there are some important caveats about this data that need to be addressed first.

1. As shown in the bar graph above, Kentucky leads in the count of total permits issued. That being said, Kentucky runs a new check on each concealed carry license holder each month, which boosts the states' numbers of annual permit issuance. This introduces bias and overrepresentation for Kentucky.
2. A 2015 Harvard study found only 60% of respondents who purchased a gun had to undergo a background check. This implies that a significant amount of data is missing or misrepresented. 
3. Some background checks are performed only for someone to obtain a concealed carry permit, and not to purchase a gun thereafter.

Despite these caveats, as mentioned previously this NICS background check data is ultimately the foremost proxy for determining gun sales per state.

From here we analyzed the ratio of permits to purchases, as this indicates the strictness of that states' purchasing laws. Essentially we wanted to see the ratio of how many permits there are to purchases for each state. The following bar graph demonstrated Hawaii and Kentucky were the highest, meaning the highest rates of purchases by permitted people occur here.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%204.png](Untitled%204.png)

With this analysis of permits and purchases completed, we then plotted the total amount of purchases per state, finding Kentucky to be the highest followed by Illinois, Texas, and California. Interestingly, Hawaii was nearly dead last, indicating they have extreme strictness in regards to purchases only coming from those with permits. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%205.png](Untitled%205.png)

Adjusting these raw amounts of purchases for population yielded a slightly different outcome in terms of gun purchases as you can see below.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_10.59.07_PM.png](Screen_Shot_2021-05-07_at_10.59.07_PM.png)

D.C. stays on top, while Hawaii and Nebraska take 2nd and 3rd, indicating the high rate of checks in each state compared to the population. **These rankings of numbers of background checks correlate closely to the previous charts on states with the highest shootings/injuries.** This is an interesting conclusion as we begin to now analyze law strictness and see how this affects the gun death rate.

## Law Strictness

---

In assessing the strictness of state gun laws, we relied on the previously cited Giffords Law Center to Prevent Gun Violence, as they developed a methodologically based Gun Law Scorecard. The center grades the states using a specific methodology, detailed as such:

>> The attorneys track and analyze gun legislation across all states, and assign points and values based on respective strengths and weaknesses, which are then tabulated and ranked, and then finally, assigned letter grades. Sometimes, states are tied if their tabulation added up to the same score for their law strictness. After assigning each state a grade, Giffords ranks each state from 1 to 50 in terms of strictness. 

Using this ranking of gun law strictness from Giffords, we created a geographic visual to analyze the law strictness and compare it to the gun death rate map shown earlier. **The maps possess striking similarities to each other as you can see below.**

Law strictness rank (Scale 1 to 50): 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/law_strictness_ranking.png](law_strictness_ranking.png)

Gun Death Rate Map:

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/gun_death_rate.png](gun_death_rate.png)

Visually, the correlation is visible between strict gun laws and their effect on gun death rates. The states that lead in gun death match Gifford's law strictness rank nearly perfectly. Below is a scatterplot demonstrating this correlation.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%206.png](Untitled%206.png)

This shows that stricter gun laws are correlated with lower gun death. The top 30 highest gun death rate states all are ranked among the weakest in terms of their strictness (C- to F range on the Giffords Grading) except for Maryland. In constructing our model, this correlation is key to selecting features in order to construct an accurate model. Following this analysis on law strictness, we wanted to observe public sentiment on gun control by analyzing data from Twitter.

## Twitter Sentiment Analysis

---

In order to augment our analysis, we considered incorporating tweets on gun violence and gun control to produce a sentiment score to use as an added feature in our model. In doing so, we hoped to find some correlation between public sentiment and the gun death rate. 

To get this data, we applied to obtain a Twitter Developer License. This allowed us to access a wide variety of twitter data, including tweet content, engagement (retweets, likes), geolocation, and comments. 

Twitter is a powerful data source, but before we could process any tweets, we needed to clean up the text in order to remove any special symbols such as hashtags and mentions. We applied this cleaning to the entire dataset, which gave us good baseline text to perform sentiment analysis on. 

We then defined two functions: the subjectivity and the polarity of a tweet. 

Subjectivity is a measure of how subjective or factual a statement is from a range of 0 to 1, where 1 is a more subjective statement. Polarity is a measure of how positive or negative a statement and is on a scale of -1 to 1 where -1 is the most negative. 

Ultimately our efforts to use Twitter did not yield meaningful results. First, out of a sample of 50,000 tweets, only 1800 contained geolocation data. Furthermore the data was heavily concentrated in densely populated states such as California and New York. For smaller states such as Wyoming, there were often only a couple of tweets which contained relevant information. 

Second, our methods for finding sentiment and polarity were not generalized to conversations about gun control, and thus often misclassified the tweets. This could be solved with a more robust implementation of a sentiment classifier, but given the difficulties we had in scaling the data, we ultimately decided to measure public sentiment in a different manner. 

**We decided to use a ranking from the New York Times**$^5$ about how liberal or conservative each state was during the 2016 presidential election. The scale ranges from 1 to 5, 1 being the most liberal and 5 being the most conservative. Since public sentiment does not always correlate with the strictness of a state's gun laws, we hoped this would be a meaningful feature in our dataset. 

Having exhausted all potential data sources to diversify our feature pool, we finally moved into developing the model itself to predict gun violence. We started first with feature selection, employing various correlation matrices to sort and select features.

## Feature Selection

---

We started the process of selecting features by developing correlation matrices for different aspects of the data to understand how they correlate to the gun death rates. This would allow us to identify more easily which features would be most significant to create a more precise model. The first matrix we used was for the background check data from the NICS. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%207.png](Untitled%207.png)

No stark correlations appeared with Gun Death Rate, but there was some correlation between "prepawn_handgun" and "prepawn_long_gun" which are not useful features for predicting the gun death rate. The FBI defined these as, "Background checks requested by an officially-licensed FFL on prospective firearm transferees seeking to pledge or pawn a firearm as security for the payment or repayment of money, prior to actually pledging or pawning the firearm." This is a fairly specific case of background checks and this slight correlation is thus not very meaningful or relevant to our model. 

The next correlation matrix we used was for analyzing how law strictness correlates to the gun death rate.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_8.31.42_PM.png](Screen_Shot_2021-05-07_at_8.31.42_PM.png)

We see a high correlation between how strict state laws are and the gun death rate. This is a positive sign for our model as **it demonstrates the significance state law strictness can have on lowering the gun death rate**. There is also a strong negative correlation between "Grade 2019" and Gun Death Rate, which is unsurprising since more strict laws were given a higher grade, which subsequently correlated to a lower death rate. 

We also see a high positive correlation between our public opinion column and the gun death rate, indicating that public opinion is another useful variable for our model. One caveat is that it is also highly correlated with the law rank and grade, leaving the potential for multicollinearity issues in our model. 

Next, we looked more into how the number of each type of gun control law correlates to the gun death rate which yielded some very interesting findings. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%208.png](Untitled%208.png)

The notable negative correlations are "Required Reporting of Lost or Stolen Firearms" and "Safety Training Required", along with "Firearm Sales Restrictions". All of these are stronger than -0.5 and indicate these laws are statistically significant when more of them are passed to reduce the gun death rate. These will help in strengthening our model and helping draw more nuanced conclusions on which types of gun control laws in particular help reduce gun violence.

### Correlation between variables

To expand on the big correlation matrices, we wanted to find which types of gun laws have the strongest correlation with reduced death rates. We found that the laws most likely to reduce the gun death rate include laws pertaining 

1) reporting lost or stolen firearms

2) required safety training

3) prohibiting certain gun possessors. 

The following boxplots and scatter plots show the spread of death rates for states that have different numbers of laws in each category.  

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_8.54.42_PM.png](Screen_Shot_2021-05-07_at_8.54.42_PM.png)

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_9.22.53_PM.png](Screen_Shot_2021-05-07_at_9.22.53_PM.png)

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_6.40.31_PM.png](Screen_Shot_2021-05-07_at_6.40.31_PM.png)

We found that for some law categories, there is very little correlation between gun death rate and the number of laws in that category. Some of these types of laws include laws pertaining to 1) waiting periods between purchasing a firearm and taking ownership of it and 2) open carry laws. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_6.47.24_PM.png](Screen_Shot_2021-05-07_at_6.47.24_PM.png)

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_9.01.46_PM.png](Screen_Shot_2021-05-07_at_9.01.46_PM.png)

There are some law categories that are positively correlated with the gun death rate, meaning that the more laws that exist in that category, the more deaths per population. One of these categories is laws pertaining to carrying a concealed weapon. The following boxplot shows this positive correlation. This is an example of one weakness of our data that we mentioned previously, that the sheer counts of laws in each category are not a perfect proxy for the strictness and detailing of the laws themselves. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_8.55.34_PM.png](Screen_Shot_2021-05-07_at_8.55.34_PM.png)

In addition to the analysis of gun law categories, we plotted correlations between gun death rate and several other variables. The following plot shows the gun death rate plotted against the ratio of permits to total purchases. We hoped that this metric of permits/purchases would be an additional metric with which to measure gun law strictness and adherence. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_9.17.20_PM.png](Screen_Shot_2021-05-07_at_9.17.20_PM.png)

This analysis shows that there is a slight negative correlation, with many points clustered around (0.1, 15). This is an indication that there may be a correlation between Gun Death Rate and Permits — as the number of permits increases, gun death rate goes down. 

With this feature selection analysis completed, we will now shift into how we developed our model based on this understanding and how we fine-tuned it to create a predictor of gun death rate based on the data we collected. 

## Model

---

Our project is studying the various factors contributing to gun violence in the U.S. We executed this study by trying to find a correlation between a numerical variable (the gun death rate) and the strictness of gun laws in that state. Additional variables that we used to predict the gun death rate include population, the total number of people injured by gun violence in that year, and the number of gun-related background checks done in that year. The background check data is used as a proxy for gun purchases since there is incomplete data about gun purchases across states. 

Since we are predicting on a numerical variable using other numerical variables as features, we can model this as a regression problem using supervised methods. We utilize several models, employing feature engineering to improve our results.

### Measures of Significance

For our models we will be using two main measures to explain the validity of our model. 

*Root Mean Square Error*: RMSE is a measure which calculates the average distance between the predicted points from the target points. This is also a measure of the spread of the residuals. A good model will have a relatively low RMSE scaled to the target variable, as the predicted points will be more concentrated around the line of best fit. 

*R^2:* R^2 is a measure of the statistical correlation between two variables. In this case, we are measure our model's predicted points on the target variable; in other words, the correlation between the model and the target values. R^2 is measured from 0 to 1, and a value closer to 1 is considered. 

### Initial Attempt at a Model

Our first attempted model illustrated the validity of linear regression for the dataset. This iteration allowed us to better select data and features based on the findings. We started with simple linear regression and used Principal Component Analysis to determine the collinearity of the dataset.

### **Linear Regression**

Linear Regression is a type of predictive analytics that can help us predict whether a certain set of predictor variables can reasonably determine an outcome. In this linear regression model, we are using the shootings table to predict gun death rate outcomes.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Untitled%209.png](Untitled%209.png)

As we can see, the graph of the residuals appears relatively random around the $y=0$   axis. This is an important finding, as it suggests that linear regression is an appropriate choice of model. We can see that the spread in the horizontal direction is relatively even, and there is a slight outlier towards the bottom. This also suggests that accuracy can be improved, as the most likely scenario explaining this is that a variable is missing in the model (more on this later). 

### **PCA and Feature Selection**

PCA is an algorithm used to perform dimensionality reduction on the dataset. It's often used to speed up training algorithms by encapsulating most of the data in a lower dimension and can also help show which features are most impactful in our model. Ideally, this approach will help reduce overfitting and lower the variance in our model caused by the presence of too many features.

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_9.15.10_PM.png](Screen_Shot_2021-05-07_at_9.15.10_PM.png)

Above, we can see the explained variance in the data according to PCA. This graph shows us the relationship between explained variance and the number of components. In order to explain 80% of the variance in our dataset, we only need to use around 10 components. Based on this finding, we can specify specific features that we want to use to test our model. 

In creating our models, we selected features according to the correlation matrix shown earlier. These included : ['Year', 'Laws Rank', 'Injured in Shootings Total', 'Grade 2019', 'permit', 'totals', 'POPESTIMATE2015', 'Gun Death Rate']. 

---

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_9.25.09_PM.png](Screen_Shot_2021-05-07_at_9.25.09_PM.png)

As we can see from the plot of the residuals, the values appear to have a linear pattern to them. They aren't randomly scattered around the horizontal axis and instead have a negative slope, suggesting that collinearity is still an issue in our model. The RMSE for this model was 3.78 and the $Rˆ2$ was 0.60. This high $Rˆ2$ indicates that the model is working fairly well, but requires finer tuning and iteration. 

### Tuning and Final Model

The key finding from our initial attempt at a model was that our original data had a lot of collinearity with gun violence rates, and the best features included gun law data and sales. Based on these findings, we collected new data that included the number of laws regarding various parameters such as background checks, open carry laws, minimum age, safety training requirements. 

### **Linear Regression**

After incorporating the new data from above, we used PCA to determine the lowest dimensionality to explain the variance using the gun laws as the highest weighted features. Using these new parameters, we were able to fit a linear regression with a validation RMSE of 2.7487524616344285 and $Rˆ2$  of 0.71. The residual graph also showed promise, as it was very randomly scattered around the horizontal axis with a good spread. There was only one outlier in the bottom right which did not impact the results too much but would be worth looking into in future studies. 

![Legal%20Studies%20123%20-%20Data%20Investigation%20Project%20Pre%200406a2134e08425da0a9294029e10c30/Screen_Shot_2021-05-07_at_9.46.45_PM.png](Screen_Shot_2021-05-07_at_9.46.45_PM.png)

### **Testing the Model**

Once we were happy with the model on the test results, we ran the model on the test set. Ultimately our test RMSE was 1.84 and our test $R^2$  was 0.906. Our model was highly correlated with the target variable, gun violence rates, and our error was relatively small. Our model is pretty accurate! We were able to fit a good linear model to predict gun violence rates in states using the metrics we identified. For more discussion on the impact of these findings, see the conclusions section below. 

## Conclusions

---

From our analysis, we have reached several conclusions. 

The first is that **states with stricter laws have significantly lower death rates** than states with less strict laws. More specifically, we see a high correlation between several types of gun laws and a low death rate. The laws with the highest correlation include laws that target prohibited possessors, required safety training, required reporting of lost or stolen firearms, and additional sales restrictions. 

This leads us to recommend that **states increase their laws in these categories specifically to see a decrease in gun-related deaths**. For the most part, these categories are effective at preventing weapons from falling into the wrong hands, and consequently, it makes intuitive sense that these laws also decrease the death rate. These categories are likely to float in the legislature, as they are also amenable to the viewpoint of both gun-reform activists and pro-gun citizens. Specifically, these laws do not prevent the average person from gaining access to a gun if they so desire. They do however make it more difficult for ill-fit individuals to access guns.

Conversely, from our brief and incomplete analysis, we found that there are several categories that do not appear to have much of an impact on the death rate by firearms. Laws pertaining to openly carrying firearms and waiting periods before a gun owner may have access to their gun have **almost no impact on the gun death rate**. Perhaps then, states may choose to relax these restrictions, which the data suggest would likely have little impact on the death rate by firearm.

We also found several law categories that were positively correlated with the death rate, indicating that in some cases, **more restrictive laws lead to more deaths by gun**. Though we are skeptical logically of that conclusion, our data does point to this conclusion, especially for the category of carrying a concealed weapon. This correlation in our data is an indication that our sample size is relatively small (n=50), and that the **number of laws in a category is not a proper approximation** for the strictness of a state's laws. 

For our problem, **a linear model with PCA dimensionality reduction proved to be the model with the best results.** We were able to achieve the highest r^2 score and lowest RMSE with this model. However, our model was still far from perfect. This indicates that there are many more factors that influence gun death rates than we can account for in this analysis. Many social and institutional factors have a great influence on the gun death rate, and as a result, it is impossible for us to have definitive conclusions and recommendations for lawmakers given our work on this project. 

## Risks

---

There are several risks inherent to our driving question and work so far. We have outlined several of them below. 

We must consider the ethics of misrepresenting states based on actual shooting occurrences, rather than based on proportionate occurrences to population. For example, given a large population size, California would likely have more shootings than a rural state. Therefore, if we don't visualize this using population data, California will be misrepresented at the top.

This data encapsulates shootings reported, not shootings and crime that actually occurred. This likely creates a misrepresentation where uncounted crime creates data skew.

This data measures counts based on "killed" and "injured". It is not updated with people who died later (months, years, etc.) from complications.

The Giffords methodology for assessing and ranking each state's gun laws is not made public and instead only decided and known by their team of attorneys. Gaining more insight into how they assign points and tabulate all the states could be supplemented with methodologies from other sources to strengthen the legitimacy of this. 

## Next Steps for Further Investigation

---

Given more time to dig deeper into this project, there are several opportunities for future work to consider. Collecting data is the most difficult part of analyzing this topic, so reaching out to government agencies/businesses to gain access to private data, potentially paying for it, would help a lot in strengthening correlations and solving some of our sample size issues. The background checks conversion to gun purchases could be tackled in a more complex way as well. A complex Swedish research paper titled "The Small Arms Survey" introduced in a 98 page paper a methodical way to convert background checks in a 1.1 ratio, but implementing this would require far more time and complexity than the scope of this class. Further, our data was limited by the time frame of some datasets, as the Gun Violence Archive only ranged from 2014 - 2016. We did have updated, larger timeframe data for our background checks and strictness data, meaning more range on the data from the Gun Violence Archive (number of shootings/killings/injuries) could be useful in contextualizing with the other datasets more.

## Questions for Researchers and Investigators to Consider Further

---

1. Evaluate the biases of this model
    1. How can the models be adjusted to minimize over and under-fitting?
    2. What is the false positive, false negative, true positive, and true falsie rate?
    3. Where is the data prejudiced?
    4. How can we reduce multicollinearity in our models?
    5. How does this data misrepresent certain states?
2. How can a shorter or longer time period evaluated affect predictive results? Will certain events (ex: the polarized election of Donald Trump to Presidential office) affect results? How do events and media coverage change sentiment? How often do polarity and public viewpoint on guns change?
3. How can gun purchase data and background checks provide another layer of information? Since general sales data is difficult to draw conclusions with, how can we take individual gun purchases and compare them with background checks to understand overall attitudes towards guns?
4. How can twitter data be a strong point in this analysis? Is it a strong enough data source to provide sensitivity analysis and polarity evidence? How can this be merged with or studied to extrapolate other sources of sentiment (Ex: polling data)?
5. How can this study be used to understand the relationship between polling and Twitter activity and general sentiment? How strong or weak are the ties between Twitter data and public ideology? Where does Twitter overrepresent voices? Where does it underrepresent voices?
6. What are the applications of a study like this that can be used to further and propel various safe carry narratives? What are the ramifications of a study like this on overall public sentiment towards gun laws? 

## Appendix

---

Restrictions that help reduce gun death rate: 

[These Three Firearm Restrictions Could Help Reduce Gun Deaths in Your State](https://www.rand.org/research/gun-policy/firearm-mortality.html)

## References

---

1. Archive, G. (2016, November 27). Gun violence database. Retrieved May 07, 2021, from [https://www.kaggle.com/gunviolencearchive/gun-violence-database](https://www.kaggle.com/gunviolencearchive/gun-violence-database)
2. “America's Gun Culture in Charts.” BBC News, BBC, 8 Apr. 2021, [www.bbc.com/news/world-us-canada-41488081](http://www.bbc.com/news/world-us-canada-41488081).
3. Giffords Law Center's Annual Gun Law Scorecard. (n.d.). Retrieved May 07, 2021, from [https://giffords.org/lawcenter/resources/scorecard/](https://giffords.org/lawcenter/resources/scorecard/)
4. BuzzFeedNews. (n.d.). Buzzfeednews/nics-firearm-background-checks. Retrieved May 08, 2021, from [https://github.com/BuzzFeedNews/nics-firearm-background-checks](https://github.com/BuzzFeedNews/nics-firearm-background-checks)
5. Cherney, S., Morral, A., Smucker, S., & Schell, T. (2020, September 4). Development of the RAND State Firearm Law Database and Supporting Materials. Retrieved May 07, 2021, from [https://www.rand.org/pubs/tools/TLA243-2.html](https://www.rand.org/pubs/tools/TLA243-2.html).
6. 2016 Presidential Election Results. (2017, August 19). New York Times. Retrieved May 7, 2021, from [https://www.nytimes.com/elections/2016/results/president](https://www.nytimes.com/elections/2016/results/president)

# Data Sources

1.   Detailed info about mass shootings - [[Gun violence database](https://www.kaggle.com/gunviolencearchive/gun-violence-database)]

2.   Law strictness rankings - [[Giffords Law Center](https://giffords.org/lawcenter/resources/scorecard/) ]

3.   Law categories by state - [[RAND State Firearm Database](https://www.rand.org/pubs/tools/TLA243-2.html)]

4.   Public opinion - [[NYT 2016 Election Results](https://www.nytimes.com/elections/2016/results/president)]

5.   Twitter data - Twitter

6.   Background check data - [[NICS Firearm Checks](https://github.com/BuzzFeedNews/nics-firearm-background-checks)]

7.   Population data - [[census](https://www.census.gov/data/datasets/time-series/demo/popest/2010s-state-total.html#par_textimage_1873399417)]

8.   Shapefile of states for maps - [[census](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html)]