---
layout: page
title: Group 05
nav_order: 10
description:
---

# The Cross-Country Socioeconomic and Political Determinants of Terrorism


Authors: Megan Wang, Elliot Choi, Can Sarioz, Sally Nelson


## **1.1 Introduction**

What is the country-level macro socioeconomic, ecological, and political determinants of terrorism? Which factors have the largest sway in informing the level of terrorist activity within a country? Although terrorism is largely depicted as politically motivated in popular media, research in the past has shown that socioeconomic factors also play a mediating role in the degree of terrorism seen across different countries. Namely, low socioeconomic status often serves as lower opportunity costs for acts of terrorism to be instigated. Our group seeks to evaluate which of these factors: political vs social vs economic, bears the largest weight when it comes to the degree of terrorism within different countries globally. We hope to fit our model onto countries with which we did not use to train our model and evaluate whether the largest determinants of terrorism are stable across time and countries. The vast majority of terrorist attacks, even by groups that represent the most serious foreign threat to the US, mostly attack domestic targets in their home country. By understanding the determinants of terrorism on a more granular level, governments may be better informed on where their resources should be spent on development.


## 1.2 **Datasets of Interest**

### *1.2.1 “countries” dataset*
The **"Countries of the World"** dataset was downloaded from Kaggle, which lists its license as CC0: Public Domain. The dataset is made up of open-source data from the US government's CIA World Factbook. According to The World Factbook's website, "The World Factbook is in the public domain and may be used freely by anyone at any time without seeking permission."

In the "countries" dataset, each row represents a different country, and each column a different statistic related to that country. The columns include demographic and ecological information about the country, including the population, area in square miles, population density, coast/area ratio, net migration, climate, crops, birthrate and death rate. This information is important to getting a basic picture of each country. It also has data that offers insight into the socioeconomics of each country, like GDP, infant mortality, crops, agriculture, industry, service literacy percentage, and phones per 1000. 

### *1.2.2 “freedom” dataset*
The **"Human Freedom Index"** CSV dataset was downloaded directly from Cato.org, the official website of the Cato Institute. The Human Freedom Index report and dataset is an annual joint publication by the Cato Institute and Fraser Institute. The dataset is available for public domain and usage.

The "freedom" dataset includes information about personal freedoms in each country according to 79 different factors. It includes the overall freedom score of each country as well as how the country ranks in each specific category of personal freedom that the index takes into account. 

### *1.2.3 “terrorism” dataset*
The **"Global Terrorism Database"** is an open-source database containing information on terrorist attacks around the world from 1970 through 2017. The GTD is maintained by researchers at the National Consortium for the Study of Terrorism and Responses to Terrorism (START), headquartered at the University of Maryland. The GTD is available without a fee for individual uses in scholarly, educational, and research purposes. Our team downloaded the dataset from the official GTD website and followed the START's process for accessing and downloading the data. The terms of use for this data can be found at https://start.umd.edu/gtd/ and shall be rigidly followed by our team as we move forward with our analysis. The dataset was downloaded as an excel file and converted into CSV format.

### *1.2.4 “countries.geojson” dataset*
The Countries.geojson dataset is a geojson file containing geojson polygons for all the world's countries. The dataset is downloaded from datahub under the Open Data Commons Public Domain License. The original data from Natural Earth, a community effort to make maps with cartography or GIS software, is public domain. The geojson file contains the common name for the country and the 3-letter ISO code of the country. We merged the goejson file with our country’s dataset by matching ISO codes.

We aggregated the data so that we could look at the number of terrorist attacks in each country. Our final dataset includes all of the information necessary to answer our research question in one place. It has a line for each country and includes the features related to personal freedoms and ecological and socioeconomic factors, as well as the number of terrorist attacks that have taken place in said country. 

### *1.2.5 Preliminary Data Analysis*
After looking at the data, it seems as though there have been relatively few terrorist attacks in most countries – there are only a few countries who have experienced more than 500 attacks since 1970. The data shows that countries in the Middle East have experienced the highest number of terrorist attacks collectively. The most common type of terrorist attack is bombing. This aligns with the portrayal of terrorism that is most robust in the media. Tracking terrorist attacks by year indicates that the number of terrorist attacks each year has increased significantly since 1970, especially since 2008 – there is a peak in 2014. Mapping terrorist attacks according to type allows us to get a better visual understanding of where attacks tend to occur, and what types are most common in which place. There are definitely some limitations to the data that we are using, which is important to consider when conducting analysis. Though we sourced the data that we are using from reputable institutions, all data is subject to human biases, which is important to remember when conducting analysis. A primary example is the understanding of what constitutes a “terrorist attack.” Whose definition of terrorism are we using, and what types of events are classified as terrorist attacks? Typically in the media, violent events that occur in the Middle East are more often considered “terrorist attacks” than similarly violent events in Western Europe or North America. This is reflected in our data – the region with the largest number of terrorist attacks is the Middle East. This may be rooted in factual differences in the number of terrorist attacks, but it is also important to think about how bias in the categorization of terrorism may play a role. Bias is also important to consider when looking at a particularly subjective topic like personal freedom. The process of assessing personal freedom seems quite rigorous, but it is important to consider how personal and media biases may play a role in influencing so-called "freedom" scores. What does this "freedom score" actually measure – sure it is based off of a multitude of different factors, but what does that actually mean?


## **1.3 Methodology**

### *1.3.1 High-level Overview*
As part of the modeling and prediction portion of our project, we plan to conduct a train/test split of our 300+ countries and run regression models using our various socioeconomic and political predictors. Through techniques such as PCA, lasso/ridge regression, correlation matrices, and classification matrices, we hope to answer our research question of which determinants are mostly helpful in predicting the level of terrorism in a given country, and whether these determinants are stable across time and countries.


### *1.3.2 Process*
For the purpose of making our data accessible for modeling purposes, we cleaned up our freedom dataset to include only 1 row per country (rather than country-year pairs for our rows) in order for easy merging with our countries dataset (which includes only 1 row per country). We believe that aggregating our freedom dataset by mean values for each country is a reasonable strategy for matching our 2 datasets since the countries dataset also includes values aggregated over numerous years. We dropped the "year" column of our grouped table and the grouping mechanism removed the categorical columns of "region". We also dropped columns in our freedom dataset with null values, such that null values (or placeholder values) will not conflict with the modeling component of this project. This reduction in the number of potential features for modeling also helps us prevent potential overfitting that can occur when too many features are used in predictive models. Note that this does not result in a reduction in the # of rows (162 rows) present in our freedom dataset, only a reduction in the # columns (from 112 to 65).

### *1.3.3 Caution*
The data cleaning process in which we aggregated values over years may present potential problems if qualitative variables such as (rule of law, size of government, and religion) are highly volatile and unstable over time. However, given that we are operating off the hypothesis that country-level predictors at the macro level are relatively stable over time and their relationships with terrorism are stable over time, this should not post too major of an issue when combined with the fact that we are focused more on defining the features impacting terrorism (rather than changes in terrorism over time) and that our lasso/ridge models will help with feature selection. When referring to some existing literature on the socioeconomic predictors of terrorism, we found that the best practices employed by the majority of researchers is to operate on country-pair macro-level variables aggregated over numerous years (rather than specifying the year-over-year changes in said values).



## **2.1 Exploratory Data Analysis**

We first examine terrorism counts by region and country as well as attack and target types:

<img src='Countries A.png'>

The Middle East & North Africa has the highest # of terrorist incidents, followed closely by South Asia. South American, Sub-Saharan Africa, and Western Europe distantly trail behind in the # of terrorist incidents, while East Asia, Central Asia, and Australia/Oceania have very little counts of terrorism incidents by comparison.

<img src='Countries A1.png'>

The top 15 countries in count of terrorist activities are mostly in the Middle East & North Africa region or South Asia, which makes sense if we look at the graph above of total terrorist activities by region. However, interesting to note is the the United Kingdom and Spain also fall in the top 15, though these two countries fall in the Western Europe region, which is only the #5 region with the most terrorist activities.

<img src='Type A.png'>

The most common types of attack are bombing/explosions, followed by armed assault and assassination.

<img src='Target A.png'>

The most common targets are private citizens & property, followed by military, police, governmenet, and businesses. 

<img src='Year A.png'>

As seen above, it appears that terrorism has risen dramatically from 1970 to 2018. In general, terrorism follows up upward trend, with a minor dip in the 1990s, sharp rise after the 2000s, and a minor dip after 2014.

<img src='Region B.png'>

The global trend in terrorism is still followed when we break down terrorism count by region - there's a spike across all regions in the 2000s. In particularly, the Middle East & North Africa region and the South Asia region had the largest absolute and relative rise in terrorism when compared to itself over time and the rest of the world.

<img src='Type B.png'>

After 2005, there is a significant rise in the usage of bombing/explosion as an attack type in terrorist incidents. This is concerning since bombing/explosions tend to result in a higher number of casualties. This calls into question whether this rise is attributed to greater accessibility to explosive materials to terrorist, or whether other factors are changing the motives behind terrorist incidents.

<img src='Target B.png'>

As we've seen in the graphs above, the target types in terrorist attacks has also experienced some major shifts post 2000s. Private citizens & property has seen an overwelming increase as the targets of terrorist attacks, even relative to the general rise in terrorist attacks post 2000. This combined with the fact that we can see a similar change in the composition of attack types and region implies that there is a fundamental shift in terrorism post 2000.

<img src='Map A.png'>

From the map above, we can see that terrorism does tend to be clustered at specific locations, even within the boundaries of each country. For example, we can see that in South America, terrorist incidents tend to be clustered along the west coast, while in South Asia, terrorist incidents tend to be clustered in the upper-left and upper-right corner.These are all areas with the highest urban density and population growth, which confirms the findings of previous literature on terrorism.Although we mapped out only 3000 sampled points from the global terrorism database for the sake of run time, we can also see our previous findings confirmed that terrorism is highest in the Middle East & North Africa region and the South Asia region.

<img src='Map B.png'>

As seen above, the most common attack type is overwhelmingly bombing/explosions, as seen above in the heavy concentration of dark blue dots. However, it does appear to be the case that the usage of certain attack types are geographically linked. For example, in Nigeria and Mexico, the preferred method of attack is armed assault (in pink), while facility/intfrastructure attacks mostly occur in India or Europe (particularly the Netherlands and UK).

<img src='Region C.png'>

There appears to be no significant relationship between population density and terrorism count, though it is notable that terrorism count is very low (or close to 0) for any countries with a population density greater than 400 per sq. mi.

<img src='Region D.png'>

We can see on the scatterplot above that terrorism count tends to be higher the lower the GDP (USD per capita) is for a given country. Beyond the threshold of ~$10000 GDP per capita, terrorism count significantly drops off.


<img src='Region E.png'>

Significantly, high terrorism counts are only seen in countries with a net migration index close to 0. The more a country's net migration moves towards the opposite ends of the spectrum of values, the closer their terrorism count is to 0.

<img src='Region F.png'>

As seen above, the highest terrorism count countries tend to cluster around where the range of coastline (coast/area ratio) values is below 5. Landlocked countries (or those who are more landlocked relatively to others) tend to have higher terrorism counts.

<img src='Region G.png'>

With the exception of a few outliers (the top 4 countries with the highest terrorism count), higher terrorism counts tends to be more prevalent in countries with greater litearcy % values.

<img src='Region H.png'>

Higher terrorism counts tend to be cluster closer to where the personal freedom - rule of law index is lower, specifically around the range of 3 to 5.

<img src='Region I.png'>

There appears to be a minor positive relationship between economic freedom - legal regulatory costs and terrorism count.

<img src='Region J.png'>

There appears to be a minor positive relationship between economic freedom - government size and terrorism count.

<img src='Region K.png'>

<img src='Region L.png'>

<img src='Region M.png'>

There appears to be a positive relationship between economic freedom - standard deviation of inflation and terrorism count. The highest terrorism count observations are all found where the standard deiation of inflation is >9.

<img src='Correlation Matrix.png'>

We can see that a lot of the features are usually positively correlated with each other. However, we can say from the scale that none of the correlations are very strong. In case of the terrorism count, we can confirm some of our intuitions from our previous scatter diagrams. The most of the features have a negative relationship with terrorism count, which would be expected. The rate of the correlation would be one of the main focuses of our future analysis.


## **3.1 Model Selection**

### **3.1.1 Ridge Model** 

The ridge regression model is one of the most common ML prediction techniques for linear problems. Ridge regression estimates the coefficients of multiple-regression models in scenarios where independent variables are highly correlated. It addresses some of the multicolinearity problems that occur in OLS models by imposing a penalty on the size of the coefficients. The ridge coefficient minimizes using a squared term.

A pro of ridge regression is that is performs well when you have a large number of columns and a small number of rows, this is clearly an issue that our dataset has (particularly after our train/val/test split. An additional pro is that ridge regression is a regularized model, which explicitly helps avoid overfitting, which is especially beneficial when our training set is so small. The ridge estimator is especially good at improving the least-squares estimate when multicollinearity is present. We know from our EDA that there is a degree of multicollinearity in our dataset, as some of our explanatory features are correlated with others (see our correlation matrix in EDA). However, ridge cannot zero out coefficients, only focus on more important features. This may be negative since we have more features than observations. This brings us to our next model, lasso, which DOES allow feature selection.

We chose to use ridge because of the structure of our dataset – many columns, fewer number of rows. Additionally, we really wanted to avoid overfitting, which is of particular concern given the small size of our training dataset.

### **3.1.2. LASSO Model** 

The LASSO model is quite similar to ridge regression – it is a regularized regression technique. It differs in its regularization method. Instead of using a L2 (or squared norm) as penalty, lasso regression uses a L1 (or absolute value norm) as penalty for coefficients. It shrinks data values towards a central point, like the mean, and produces simple sparse models with fewer parameters. Like ridge regression, it is often preferred to simpler OLS regression methods because regularization helps avoid overfitting.

A pro of lasso regression is that it selects features for you by shrinking co-efficients towards zero. When we have high dimensionality and high correlation in our dataset, L1 regularization would be preferable because it penalizes less important features and makes them 0 (aka algoirthmic feature selection), making your model more robust than L2 regularisation. Lasso regression may be an appropriate choice for our problem since we have so many features and do not exactly know which is most useful. We likely should not be creating a model with 90-something features (which is how many explanatory variables we have), so lasso may be useful in helping to make our model more simple and robust by selecting our features for us. Lasso tends to do well if there is a small number of significant parameters and the others are close to zero (ergo: when only a few predictors actually influence the response). From an interpretability standpoint as well, we expect to see only a few smaller # of parameters have the highest impact on terrorism count, thus, lasso makes sense since it prioritizes the most important parameters.

### **3.1.3 Elastic Net Model** 

The Elastic Net model first emerged as a result of critique on lasso, whose variable selection can be too dependent on data and thus unstable. The solution is to combine the penalties of ridge regression and lasso to get the best of both worlds. This combination allows for a sparse model where few of the weights are non-zero like lasso, while still maintaining the regularization properties of ridge. Essentially, elastic net combines the benefit of feature elimination from lasso and feature coefficient reduction from ridge to improve the model's predictions and reduce overfitting.

A pro of elastic net is that it is useful when there are multiple features which are correlated with one another. Rather than picking one at random like lasso, it is likely to pick both. Elastic net is a compromise between lasso and ridge regression. A con of elastic net is the computational cost. You need to cross-validate the relative weight of the L1 vs L2 penalty, which increases computational cost. Additionally, it may also be the case that upon cross-validation, we find that pure lasso or pure ridge may actually present better performance. Elastic net is a good choice for our problem because it boosts the benefits of both ridge and lasso regression as mentioned above, while simultaneously bypassing some of the issues we can face in a pure lasso or pure ridge model.


## **3.2 Feature Selection**


Below, we use a correlation matrix to help select which features to use as predictors. Since we have too many features (particularly relative to the # of training rows we have), we need to focus engage in feature selection to prevent our model from overfitting. In the case of small datasets, simple models typically perform better, generalize better and avoid overfitting better than more complex models. Selecting only features that are highly correlated to our target variable, terrorism count, may help improve our model's effectiveness, and reduce any overfitting that may occur. A correlation matrix is a helpful method of visualizing how strongly each feature is related to the Y variable that we are looking at – in this case, terrorism counts. We then choose features based on how strong their correlation is to terrorism counts. Because we are interested in values that are both positively and negatively correlated to terrorism count, we selected features that have correlation coefficients with absolute values greater than .15 – a relatively strong positive or negative correlation.


<img src='Correlation Matrix copy.png'>

It appears that in general, the scatterplot is much darker than it is light. This implies that more variables are more negatively correlated with one another than positively correlated with one another in general. If we focus on terrorism count, our dependent variable, we can see that its correlation with the explanatory variables generally yields a purple color (which is colorcoded as around -0.25 in correlation) or pink color (which is colorcoded as around 0 in correlation). This means that our explanatory variables are generally only slightly negatively corrrelated to our target variable, or not correlated at all. There are a few explanatory variables that are correlated to terrorism count with a dark purple/black color, which indicates a stronger negative correlation. Explanatory variables that are more strongly correlated to our target variable may be better choices of features.

## **3.3 Modelling Training and Test**


<img src='Training Model.png'>


<img src='Test Model.png'>


## **3.4 Modelling Results and Analysis**



In our original regression models without any feature selection, the elastic net model had the lowest training RMSE while the lasso model had the lowest validation RMSE. When we used a correlation matrix to help us select features, the lasso model was overwhelmingly the winner, yielding the second lowest training RMSE by a very small margin and the lowest validation RMSE. When we used sklearn's RFECV implemetation, we saw that the elastic net model had the lowest training RMSE while the lasso model had the lowest validation RMSE. Following on with these findings, we would expect the lasso model to generally be the best performing model when we evaluate our models on a test set.

However, interestingly enough, this is not exactly the case. Amongst our 9 models, the RFECV ridge model yielded the lowest RMSE, followed by the ridge model with correlation matrix feature selection. If we look at the average RMSE of each model type however, we can see that the elastic net models actually had the lowest average RMSE while the ridge models had the highest average RMSE.

What this means from an actionability standpoint is that our model selection of which model has the best performance is not perfectly clear cut. It depends on whether we want to prioritize lowering our training, validation, vs testing RMSE, as all 9 models have disparate performance results. If we want our 'best' model to generalize well to new unseen data, then choosing the model with the best testing RMSE may be the best choice.

However, we should also keep in mind that RMSE is just one of the possible scoring methods we can use when it comes to evaluating regression models. Alternatively, we could also consider using r2 scores or variance explained scores instead.


<img src='Model Results and Analysis.png'>


In our original regression models without any feature selection, the elastic net model had the lowest training RMSE while the lasso model had the lowest validation RMSE. When we used a correlation matrix to help us select features, the lasso model was overwhelmingly the winner, yielding the second lowest training RMSE by a very small margin and the lowest validation RMSE. When we used sklearn's RFECV implemetation, we saw that the elastic net model had the lowest training RMSE while the lasso model had the lowest validation RMSE. Following on with these findings, we would expect the lasso model to generally be the best performing model when we evaluate our models on a test set.

However, interestingly enough, this is not exactly the case. Amongst our 9 models, the RFECV ridge model yielded the lowest RMSE, followed by the ridge model with correlation matrix feature selection. If we look at the average RMSE of each model type however, we can see that the elastic net models actually had the lowest average RMSE while the ridge models had the highest average RMSE.

What this means from an actionability standpoint is that our model selection of which model has the best performance is not perfectly clear cut. It depends on whether we want to prioritize lowering our training, validation, vs testing RMSE, as all 9 models have disparate performance results. If we want our 'best' model to generalize well to new unseen data, then choosing the model with the best testing RMSE may be the best choice.

However, we should also keep in mind that RMSE is just one of the possible scoring methods we can use when it comes to evaluating regression models. Alternatively, we could also consider using r2 scores or variance explained scores instead.

<img src='Optimal.png'>

Assuming that we choose our RFECV ridge model as our best model since it had the lowest testing RMSE, we can see above that the # of features has narrowed 90 to 10. If we narrow in and evaluate these 10 features, we can see that 7/10 of these features are a personal freedom value from the human freedom index dataset. This implies that of our 90 total possible features, which spans across ecological, geographic, socioeconomic, religious, and political scopes, 'personal freedom' is the largest determinant of terrorism count. Specifically, our 10 features are 'service', 'human freedom score', 'personal freedom - security and safety - homicide', 'personal freedom - security and safety - disappearances', 'personal freedom - women's security and safety', 'personal freedom identity and relationships', 'personal freedom score', 'personal freedom - rank', and 'economic freedom score'. Looping back to our research question, this implies that personal freedom is the largest factor in informing the level of terrorist activity. The subsection of features within the umbrella of personal freedom shows that identity politics and security are indeed large determinants of terrorism, as expected by dominant literature and media coverage surrounding terrorism. However, the inclusion of economic freedom score and service (as % of country's economy) demonstrates that socioeconomic factors also play a role in mediating terrorism as well. This reaffirms the hypothesis mentioned in our research question that socioeconoimc status serves as an opportunity cost for acts of terrorism to be instigated.

## **4. Conclusion**

From our plotting of the data we were able to make some basic conclusions about the nature and frequency of terrorist attacks globally. The data showed that the largest number of terrorist attacks occurred in the Middle East and South Asia. Considering these results, it is important to consider the racialized nature of the term “terrorism,” and what we define as a “terrorist attack.” The United States, for example, experiences an exorbitant number of mass shootings and hate-based violence. Considering the fact that terrorism is defined as “the unlawful use of violence and intimidation, especially against civilians, in the pursuit of political aims,” it is interesting that the North America is reported to be one of the regions of the world with the fewest terrorist attacks. It is crucial to question our societal definition of terrorism when examining these results, and what role they may play in repoting that the Middle East and South Asia experience the highest number of attacks.

The data also offered information regarding the most common types of terrorist attacks as well as who is most often targeted. Bombing/explosion was the most common type of attack, followed by armed assault. In terms of who is targeted, it is most frequently private citizens and property – which aligns with the definition of terrorism cited above – followed by military, police, government and businesses. It is also interesting to consider why private individuals are targeted for political ends.

The data also offered insight into the trends relating to terrorist attacks over time. After the early 2000s the number of terrorist attacks began to increase significantly. How may have 9/11 changed global perceptions of what terrorism is and where it occurs? It is also important to think about what political events were occurring in the mid 2000s and 2010s that may have resulted in higher levels of terrorism – the rise of ISIS and U.S. destabilizing of Iraq and Afghanistan both likely played a role in the increase in number of terrorist attacks. Additionally, post-2000 attacks on private citizens and property increased in particular.

Additionally, we analyzed the relationships of different factors with the number of terrorist attacks in each country, so that we could effectively answer our research question regarding the impact of socioeconomic, geographic, political and other factors on terrorism. Aligning with our hypothesis regarding the impact of socioeconomic factors on the number of terrorist attacks, the data indicates that higher numbers of terrorist attacks are correlated with lower GDP. Beyond the threshold of ~$10000 GDP per capita, terrorism count drops significantly. Additionally high terrorism counts are seen mostly in countries with a net migration index close to 0. The more a country’s net migration moves towards the opposite ends of the spectrum of values, the closer their terrorism count is to 0.

Another notable observation is that landlocked countries tend to have more instances of terrorism. It is possible that being landlocked may be a sort of proxy for the socioeconomics of a country. Being landlocked limits trade possibilities, which may correlate to lower GDP.

With the exception of a few outliers (the top 4 countries with the highest terrorism count), higher terrorism counts tend to be more prevalent in countries with greater literacy rates. This opposes traditional stereotypes around terrorism occurring in countries with low levels of education.

Higher terrorism counts also tend to be cluster closer to where the personal freedom - rule of law index is lower, specifically around the range of 3 to 5. This confirms stereotypes regarding limited freedom and increased terrorism, but forces us to question the meaning of the personal freedom index. How might biases in definitions of terrorism be reflected in the rating of personal freedoms?

Using the Recursive Feature Elimination and Cross-Validation Ridge model (it was the model with the lowest testing RMSE), we were able to narrow in on ten features that were ultimately most correlated with the number of terrorist attacks in a country. These ten features included ‘service,’ ‘human freedom score,’ ‘personal freedom – security and safety – homicide,’ ‘personal freedom – security and safety – disappearances,’ ‘personal freedom – women’s security and safety,’ ‘personal freedom identity and relationships,’ ‘personal freedom score,’ ‘personal freedom – rank,’ and ‘economic freedom score.’ Considering the fact that seven out of ten of these features are related to the ‘personal freedom’ scores, it seems as though ‘personal freedom’ is the largest determinant of terrorist attack frequency. The fact that these scores are so pertinent demands deep and thoughtful questioning into the meaning of ‘personal freedom scores,’ especially considering that this data was sourced from the Cato Institute. The Cato Institute is a self-described “Libertarian Think Tank” that is generally respected as a relatively reliable media source. That being said, it is owned by the Koch Brothers, tends to be right-leaning, and is likely to be biased in its definitions of “personal freedoms.” The way they assess personal freedom scores is likely deeply impacted by Western hegemonic understandings of “freedom” and racialized criticisms of non-Western countries. Deeper probing into how they determine personal freedom scores is crucial in considering the results of our analysis. In the future, research should consider the ethics of freedom scores and how "freedom" is defined and ranked.

The inclusion of the economic freedom score and service as a percentage of a country’s economy demonstrates the relevance of socioeconomic factors in determining the number of terrorist attacks. This reaffirms the hypothesis that we alluded to in our research question regarding the influence of socioeconomic factors on terrorism. It also has implications for policy – anti-poverty policies and economic development may indeed also be anti-terrorist measures. These findings could influence the types of anti-terrorist policies that are put in place. It seems to make sense to center counterterrorism policy around poverty alleviation and economic stimulation based on our analysis.



```python

```
