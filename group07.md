---
layout: page
title: Group 07
nav_order: 12
description:
---

# Trends in Fatal Police Shootings and Partisan Inclinations for Presidential Elections, 2015-2020
#### by Ata Kolday, Kevin Yi, Raj Bhanushali, Queenie Shen

## INTRODUCTION

In recent years, there has been rising controversy around police brutality and the use of lethal force in detainments. Moreover, the recent presidential race between Joe Biden and Donald Trump highlighted the vast differences in political beliefs at the state and county level across the United States. Among these is often the division between Republicans and Democrats on the use of deadly force by officers. 

The conversation surrounding regulation of deadly force by police continues across the country and our group would like to investigate whether there is a link between fatal police shootings and the political inclination of communities throughout the United States. Although we may have the supposition that Republican counties would be associated with counties that have higher police shootings, a close look at the data will allow us to reject or fail to reject this hypothesis.

### Research Question
How does the partisan inclination of a county affect the frequency of fatal police shootings around the contiguous U.S. and the characteristics surrounding the police shootings?

In response, we look to develop a linear regression model that predicts the number of fatal police shootings that will occur within a given county, based on that county's majority partisan inclinations. The recent trends of increasing political polarization has transformed the political divide into a social one, where tendencies that illustrate borderline xeophobia and racism have been closely associated with Donald Trump's core voter base. 

Therefore, we propose the following hypothesis:

### Hypothesis
The number of fatal police shootings will likely be positively and significantly correlated with partisan inclinations within counties that have increasingly voted for Donald Trump during the 2016 and 2020 Presidential elections.

### Data
Our dataset on fatal police shootings recorded from 2015-2020 collects location data for each reported incident. In order to determine how location, and consequently region, are associated with fatal police shootings, we decided to use the location as the key for our dataset. With a location, we are able to merge another dataset on the location such as elections data collected in 2016 and 2020. In order to merge by location we decided to use the county as our geographic unit as it’s a unit that’s large enough to group together outliers and small enough for us to offer deeper analysis. 

For example, grouping election data by state may offer a big picture explanation, but this large of a unit would overgeneralize the associated regions of police fatalities. Since the police fatalities dataset doesn’t include counties, but includes city and state, we merged this information with a dictionary of counties and associated cities and states in order to append a column of counties onto the police fatalities dataset. From here, we joined the elections dataset on the police fatalities dataset with the key on counties. Moreover, we plan to group the frequency of police shootings by each year and counties. 

In order to model political affiliation of each U.S county, we will use a public dataset containing presidential election results for 2016 and 2020 aggregated from townhall.com, Fox News, Politico, and the New York Times. To analyze police shootings, we will use a Washington Post dataset containing information about every fatal police shooting since Jan 1, 2015. 

The Washington Post Dataset on Police fatalities is our main dataset with pertinent information towards our question on use of deadly force by officers. The information recorded provides insight into the location of fatal incident as well as details of the victim and circumstance. This additional information will provide us extensions to our analysis on the features. The location information recorded for each incident in the dataset allows us to combine this data with other data that reports data and extend our analysis. 

Our group decided to use the information in the WoPo Dataset in conjunction with county level elections data. The repository containing elections data works well with our question in understanding the political inclination of a county. Although the dataset only reports the binary republican or democrat, the information is comprehensive and provides us a structure to frame our analysis.

## EXPLORATORY ANALYSIS

### Fatal Police Shootings

In a group decision, we exclude the states of Alaska and Hawaii from this analysis. Both Alaska and Hawaii incorporate different methods of districting for their local administrative institutions, and thus, do not adhere with the "county" structure that all other states within the United States follow. Therefore, we limit our exploration and analysis into the *contiguous* U.S. States henceforth.

First, we look at how the number of fatal police shootings are distributed across all of the contiguous U.S. states and counties. 

![total_shootings_by_state.png](total_shootings_by_state.png)

We see that the total number of fatal police shootings between 2015 and 2021 is highest in California by a large margin, followed by Texas, Florida, Arizona, Colorado, Georgia, and Oklahoma. 

![shootings_state_7-2.png](shootings_state_7-2.png)
*Note*: The data includes the number of fatal police shootings upto March 2021, so the drop in 2021 should not be taken into account as a trend.

These top 7 U.S. States with the highest number of fatal police shootings each have shown different trends between 2015 and 2021. Therefore, we look at individual counties to illustrate the distribution further.

#### Heat Map of Fatal Police Shootings across Contiguous U.S. Counties, 2015-2021.
![heatmap.jpg](heatmap.jpg)

Based on the preliminary analysis, the number of fatal police shootings seem to show a relatively equal distribution across all contiguous U.S. counties, whereas there seems to be larger clusters Southern California and Arizona. Since these alone do not suggest useful indications for a potential correlation, we continue our exploratory data analysis to see whether certain demographic trends may show otherwise.

One important findings based on this analysis, however, is that we have to standardize the dataset for population, seeing that the trends shown above may not provide accurate insight into answering our research question.

### U.S. Presidential Elections, 2016 - 2020

We then explore the distribution of votes at the county level for the 2016 and 2020 Presidential elections. It might help to first get a rough sense of just how many counties voted Democrat or Republican in each of the election years.

![image-2.png](image-2.png)

In general, more counties have voted Democrat in the 2020 Presidential election compared to 2016. Interestingly, the number of fatal police shootings has seen an *overall* decrease between those years as well. Seeing that the 2016 Presidential election has occured in November, 2015, and the 2020 Presidential election has occured in November, 2019, we focus on the number of fatal police shootings in 2015 and 2019.
![Screen%20Shot%202021-05-07%20at%203.35.20%20PM.png](Screen%20Shot%202021-05-07%20at%203.35.20%20PM.png)

The trend in more counties voting Democrat is also observed at the state level during the comparison of the 2016 and 2020 Presidential elections. 

![image-3.png](image-3.png)

From these charts, we can see that while Wisconsin and Pennsylvania saw quite subtle changes in voting percentages, Michigan, Georgia, and Arizona saw quite drastic ones. In Georgia, while there was almost a 4 point percentage difference favoring the Republican candidate in 2016, it was almost completely evened out in 2020, giving Democrats just the slightest edge. Michigan's voting percentages are also notable, as the Republican percentage stayed relatively constant at just under 48% between the two elections, the Democratic percentage soared from only about 47% to almost 51%. This might indicate that while not many Republican voters switched parties, previously independent voters likely favored the Democratic candidate in 2020.

These preliminary findings from the our Fatal Police Shootings and Elections data warrant our proposed hypothesis and hypothesis-testing through a linear regression model.

## MODELING

### Merging Fatal Police Shootings and Elections by County Datasets

Using our data, we would like to predict the frequency of fatal police shootings based on the political climate of a county in the most recent federal election. In order for us to model this, we use our dataset of fatal police shootings from the WoPo and merge this dataset with the elections dataset from the Census. The common key for each of the two datasets are the state code and county name to create a unique identifier for each county. Afterwards, we convert the recorded date and time from the fatal police shooting incident into a date time object and strip the year. Doing so allows us to create a year column that allows us to segment our data for future analysis. 

With the fully merged dataset of fatal police shootings by county and the corresponding political inclination of each county, we'd like to take a deeper look into how the political inclination of each county may influence the prediction of police crimes. The major changes in national political inclination we'd like to evaluate is the presidential election years of 2016 and 2020. In 2016, the electoral college voted for a Republican president and in 2020, the electoral college voted for a Democratic president. If a model is trained with information on 2016 county based electoral data, how accurate would a predictive model be for 2020? Perhaps there may be differences due to changing political majorities of counties. This is something we'd like to explore through the creation of our model.

Our dataset unfiltered retains information on 2015-2021. In order for us to take this deeper dive, we first extract 2016/2020 and create two different datasets. As we're looking specifically at how the political inclination of a county is able to predict the frequency of fatal police shootings, we extract specific columns related to the majority political party for each year, the per point difference (raw difference in proportional vote from GOP and DEM), and frequency of police shootings for each year. However, since each county has a different number of people living in the county, it's possible that a disproportionate population may be related to a much higher, predicted fatal shooting value. As we're looking specifically at how the political inclination of a county is able to predict the frequency of fatal police shootings, we extract specific columns related to the majority political party for each year, the per point difference (raw difference in proportional vote from GOP and DEM), and frequency of police shootings for each year. 

**Fatal police shootings data in 2020 per U.S. County with federal election data appended**
![Screen%20Shot%202021-05-07%20at%203.58.48%20PM.png](Screen%20Shot%202021-05-07%20at%203.58.48%20PM.png)

However, since each county has a different number of people living in the county, it's possible that a disproportionate population may be related to a much higher, predicted fatal shooting value. To account for this, we need to standardize the fatal police shootings proportional to the county's population. In order to do this we are importing two separate datasets and merging them by the state name to create a table with county populations and associated state abbreviations to merge onto our fatal shootings-political inclination dataset. The datasets for county population are collected from the U.S. census. Furthermore, there is no PID information so the collection and use of the data conforms to ethical standards.

We've collected all of our requisite information to predict the fatal shootings per county (proportional to county population). After appending the population of each county, we create a column for the proportional fatal police shootings per county (each row in the dataframe) which we will use as the predictor (y). The features we will use for our model will be the per point difference (how far leaning a political inclination was for a county), the voting majority (republican or democrat), and the population of each county (to account for proportion).

Since the voting majority is a categorical variable, we had to convert the variable into a numerical variable to be processed by the model. A simple equation was used to create a list indicating Republican (1) or Democrat (0). In essence, we used one-hot encoding in order to simulate the categorical information in our model. This was subsequently added as a feature to our model. Other categorical information such as state and county name were dropped.

Both our 2016 and 2020 datasets were processed separately using the exact same methods in order to prepare two datasets. Our 2016 dataset will be used for train/val/test and our 2020 dataset will be used as an extra layer of validation. With the 2020 dataset, we have a change in political inclination nationally. How will these changes in political preference of counties be associated with the accuracy of our predictive model?

![Screen%20Shot%202021-05-07%20at%204.02.03%20PM.png](Screen%20Shot%202021-05-07%20at%204.02.03%20PM.png)

With the county elections data, we decided to split the data into 2 years: 2016 and 2020. By splitting the dataset into two datasets, we have a clear distinction of a training and validation model, as well as a separate dataset we can use to test the accuracy of our model as the only distinction between the 2016 and 2020 datasets will be the year information was collected. All features used to predict the model will be the same and we will have a better idea on how applicable our model is to another dataset given similar features.

### Model Training

While evaluating the efficacy of the model, we look at three criteria: 
RMSE, $R^2$, and AIC. 
- RMSE (root mean square error) is a depiction of the noise in the data; a high RMSE indicates that there is more noise and fails to account for important features in our data; therefore a lower RMSE is generally better. 
- $R^2$ represents the correlation coefficient and represents how close data is to a fitted regression line.
- AIC explains the variation with possible independent variables; therefore, this reflects 'additional information'. A lower AIC indicates a better-fit model with fewer parameters for the same amount of variation.

#### Ordinary Least Squares (OLS)

Ordinary Least Squares is a form of predictive modeling that employs linear regression models with multiple features that considers the sum of the squares of residuals - a predicted and actual value - and aims to reduce the residuals, thereby creating a prediction where the predicted value is as close to the actual value. OLS is a standard linear regression model that serves as a baseline in predicting an outcome based on inputted features.

![ols.png](ols.png)

When looking at the three measures of efficacy, the RMSE is a very low value for OLS (0.000027388042435194972). This means that there is minimal noise for the data and each feature is important to the predictability of the data. The $R^2$ correlation coefficient of 0.157 roughly indicates that 15.7% of the variation in the predictions of proportional fatal shootings around the mean. Though not very high, given the limited features in the model, a $R^2$ of .157 is sufficient to infer some positive relationship between political inclination of a county toward Republican and its proportional fatal police shootings. An AIC of 36.73 remains relatively low and indicates low variation among the parameters.

After indicating the statistical efficiency of the modeling method, we will run OLS regressions on our test set in order to further tune the model. The results of tuning our validation and test set are as follows:

```
OLS RMSE (Validation): 2.870651018902852e-05 
OLS $R^2$ (Validation): 0.1755 
OLS RMSE (Test): 2.0795939900842913e-05 
OLS $R^2$ (Test): 0.1833 
```

The results of further tuning our model through the validation and test sets shows that the fit of our model increases through further tuning and input data. Through inputting more data, we’re able to create a more robust prediction with the political climate features and fatal police shootings.

Our model has been trained on fatal police shootings around the 2016 elections cycle. If we would like to see the variation of prediction with the 2020 elections cycle based on changing political preferences we explored in our EDA, we can use the model we turned for 2016 to predict 2020 fatal police shootings and political climate. The results are as follows:

```
OLS RMSE (2020): 4.5603
OLS $R^2$ (2020): 0.1037
```

The features in the two datasets are the same. Therefore, variation in model predictability is attributed to either a change in frequency of fatal police shootings among counties or the change in political preferences of a county. The relationship between the predicted values and actual values for our 2020 dataset and 2016 model is weaker than our 2016 dataset, though we do still see a positive relationship.

## CONCLUSIONS

As expected, our data is not an exact match due to changing political inclinations within the four years between 2016 and 2020. Even still, as the features recorded are the same and the observed scale (national) is the same, our model has a decent relationship between predicted and actual values.

The features we used when creating the model are the per point difference (how far leaning a political inclination was for a county), the voting majority (Republican or Democrat), and the population of each county (to account for proportion). Together, these features are able to predict the proportional fatal police shootings to an extent. We acknowledge that there are many confounding variables that aren't accounted for in our model, therefore the predictive accuracy may not be very high. However, when we isolate the county political preference, the modeling predicted is still indicative of a relationship that can be used to increase the predictability of more robust models. 

Our goal was to examine how the political inclinations of specific counties affected the frequency of fatal police shootings. We used the county’s voting majority in the 2016 and 2020 presidential elections to identify its political inclination, the per point difference between party votes to see how far leaning a political inclination was for a county, and the population of each county in order to account for proportional differences as our model features to predict the number of fatal police shootings. We trained three different models on the 2016 election data (OLS, Ridge, and LASSO), of which OLS was the most effective. When we generalized this model onto 2020 data, we saw an $R^2$ value of 0.0957 between the observed and predicted values, which indicates a low but prevalent correlation. 

The positive $R^2$ value indicates that there exists a relationship between the number of fatal police shootings within a given county and that county's partisan preferences in the 2016 and 2020 Presidential elections. However, the model's $R^2$ value being low suggests that the predicted values were not close to the actual mean values at a desirable extent. This reasonable doubt in the existing relationship's significance is further warrented by the actual numbers of fatal police shootings in U.S. counties by political party who won the majority votes in 2016 and 2020.

![image-5.png](image-5.png)
*Note*: The charts above illustrate the percentage of fatal police shootings to the total number in years 2015 and 2019, given that those are the years that the 2016 and 2020 Presidential elections were held, respectively. 

```
Number of Fatal Police Shootings in Counties (Democrat):  880  (2015) -> 961  (2019) 
Number of Fatal Police Shootings in Counties (Republican): 2669  (2015) -> 2553  (2019) 
```

These charts and the data above show that within counties that voted Democrat between 2015 and 2019, the number of fatal police shootings **increased** from 880 to 961. Conversely, the same for counties that voted Republican shows a **decrease** in the number of fatal police shootings, from 2669 in 2015 to 2553 in 2019.

Therefore, we **accept the null hypothesis** and report **no significant correlation** between the number of fatal police shootings within a given county and that county's partisan preferences in the 2016 and 2020 Presidential elections. There could be many confounding variables unaccounted for within the scope of this analysis, particularly regarding the partisan preferences at  the county level.

From our testing results, we can also see that the model performs differently on the 2020 data it was tested on from the 2016 data it was trained on, due to the changing political landscape just within those four years. Because the model is trained specifically on data from one year, the predictions it makes for another year are less likely to be accurate. However, if the model were able to train data over a longer period of time, perhaps even coding years as a feature, it would be more applicable to make predictions for different years. With this, counties with high proportions of fatal police shootings can be identified and examined, and then further explored on a social and political level in order to delegate specific policies and reforms that can reduce police shooting fatalities.

One limiting scope of our research was the short time-frame the data was collected under, which made the range of county-level police shooting frequencies we were attempting to predict extremely narrow. Our model could potentially be improved if we expanded our dataset to include shootings that occurred outside of 2016 and 2020. Moreover, our research question aimed to explore the connection between political affiliation and fatal police shootings, but there are a multitude of additional factors that influence the rate of police shootings. Future research stemming from our findings here could explore how racial diversity, education levels, and police funding affect the frequencies of such shootings on a county level. A potential way to improve our current model would be to add some of these additional features, which would help control for any omitted variables and better isolate the effects of political affiliation. Such research may give valuable insight into why these fatal shootings occur and whether cultural and socioeconomic differences play into the way police officers react in high-stakes scenarios.

## ETHICAL STANDARDS

When collecting data to analyze, our group adhered to ethical data collection standards by utilizing publically available data compiled from well known, official sources. Our primary data source on annual Fatal Shootings by Officers was compiled by the Washington Post. The Washington Post documented this information in conjunction with FBI, CDC, and local police reports in order to create a comprehensive record of all shootings of a civilian by a police officer, in the line of duty. The data was subsequently uploaded in its raw format to Github by John Muyskens, a graphics editor at the Washington Post specializing in data reporting. However, when considering the ethics of data use, we must consider any ethical concerns in the source data itself. Within the Washington Post data, we can see that information is not anonymous. The data is glaringly personally identifiable with the name of the victim and the exact location of reported death. The inclusion of PID may be for purposes of accountability, identification, or respect to the victim, but from an analysis standpoint, this inclusion may lead to further detriments for the family of the victim with information being made available online. As PID is unnecessary for our analysis, we have chosen to omit the name column. 

Furthermore, the WoPo reports information such as signs of mental illness. Since this information is highly subjective to the officer who records this information, there may be bias and inference on these characteristics wouldn’t be grounded on any objective standards. Therefore, we’ve chosen to omit the sign_of_mental_illness. Furthermore, we changed the 'gender' column to 'sex', since the column was labeled with either M (Male) or F (Female), representing a binary structure.

Our supporting dataset on the Election Results by U.S. County was compiled by Tony McGovern, a Data Scientist at emdata.ai. The github repository was made publicly available and compiled from multiple sources including Townhall.com, Fox News, Politico, and the New York Times. The data are quantitative reports with little subjectivity and are validated through multiple sources. Therefore the raw data is pretty reliable with little concerns on ethical collection. Our application of this data also adheres to ethical standards as we use this dataset in conjunction with dataset on fatal police shootings in order to geographically segment voting patterns and fatal police force used and do not manipulate the recorded data for our perosnal use. 

Our data for localities such as City, State and County was collected by the U.S. Census Bureau and adheres to ethical data collection standards.
