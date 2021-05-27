---
layout: page
title: Group 08
nav_order: 13
description:
---
# Identifying and Predicting Trends in Hate Crimes Incidents in the United States from 2004-2019
## Conrad Chan, Juan Montoya, Maia Alviar, Yasmin Gehman

# Introduction to Research

Through this project, we sought to answer the umbrella question of **'how have hate crimes changed over time?'** as well as, **"can we use data science to predict hate crime?** Under this broad question we looked into a series of sub-questions, such as when and where the most hate crimes occur as well as which groups of people these crimes are most often committed against.

This question and research topic in general is extremely important. There are thousands of hate crimes that occur each year in the United States. Therefore, there are thousands of people who experience one of the most traumatic offenses in modern society. A hate crime is defined as a “crime motivated by prejudice based on race, gender and gender identity, religion, disability, sexual orientations, or ethnicity” (Oxford Dictionary). Essentially, hate crimes are an attack on one's identity--thus, it is pertinent to address this issue to avoid the brutal physical and mental damage that hate crimes induce. As seen throughout our project, the number of hate crimes over the past 15 years has not decreased. Though our society is progressing, we continue to exhibit the same biases and prejudices in everyday life against individuals we deem ‘different’. It is pertinent to address this question of how have hate crimes changed over time, where the most hate crimes occur, which groups of people these crimes are most often committed against and how to predict hate crimes in order to attempt to prevent them from occurring. Furthermore, as seen in recent months, there has been an increase in crimes against Asian Americans, largely due to the COVID-19 pandemic. With this in mind, it is clear to see that hate crimes are not an issue that can be ignored. We believe the only way to stop, or at least reduce, this very important issue is to bring attention to it, through exploratory and revealing projects such as this one, so that resources are invested into the prevention of hate crimes.

# Data Collection Methods

To begin this research, we sought out data on hate crimes. The source of data is the FBI UCR Hate Crimes database. This database contains files containing information on hate crimes from 2004-2019. We downloaded these files and began parsing, cleaning and combining them to create visualizations, pull statistics and eventually create a predictive model. The data from the FBI website has information on the bias/motivation, year, general location and more, but lacks things like specific location (coordinate) and party affiliation, which we felt were important features to include. So, we used geopy to find the coordinates of the general location within the UCR data. Then, we researched data on United States Presidential elections and used it to add a party affiliation to the state in question. Below explains these processes (both the initial cleaning and combining of data).

- The hate crime data can be downloaded directly from the [FBI UCR Hate Crime Statistics website](https://www.fbi.gov/services/cjis/ucr/hate-crime).
- The elections data was published on Harvard's research repository by [MIT's Election Data and Science Lab](https://dataverse.harvard.edu/file.xhtml?fileId=4299753&version=6.0).
- The hate crimes datasets we are using are open access published by the FBI UCR Program and NIBRS. Mainly, the UCR program works in generating statistics for law enforcement, however, they also publish crime statistics for public research purposes. The UCR Program has been providing these statistics since 1930. Hate crime reporting started in 1990 due to the Congress Hate Crime Statistics act.

# Data Cleaning

The 'hc_by_bias' dataset contains the offenses by bias motivation from 2004 to 2019. It was built by combining multiple raw excel tables of different statistics across different years from the FBI database. First, the data was cleaned by dropping, adding, and renaming columns as well as joining columns with different names but the same overall meaning (ex. 'Ethnicity' used in the earlys 2000s vs 'Race/Ethnicity/Ancestry' used in the 2010s). Then, columns (ex. Year) were added with the appropriate values.

The 'hc_by_agency' dataset was created using a multistep process: First, we pulled certain excel files from every year from the FBI database. These excel files were formatted with subheadings and had differing column names/values because of the 15 year difference (2004's file looked different than 2018's). With that, we pulled 17 excel files (2012 data was split into 2 tables) into a separate notebook and cleaned/reorganized the data so it could be concatenated and grouped later. The data cleaning process for this dataset included basic cleaning/reformatting (dropping/adding/renaming columns) as well as repopulating the new/renamed columns with values in different cells using a function that iterates over every row of the excel file (see other ipynb file). The excel files for each year were combined to create the final 'hc_by_agency' dataset. See cells below.

The 'hc_by_agency_sample' dataset is a sample of 'hc_by_agency' consisting of 3,945 entries with the longitude and latitude of the agencies. We used OpenStreetMap (a python tool) to get the latitutde and longitude of each instance. OpenStreetMap allows us to input a city/state/area name and get a set of coordinates back but it takes a very long time for the requests to go through and there is a rate limit as well as an overall limit of requests from a single person, thus, we initially pulled a 4000 iteration sample.

The 'election' dataset is from a Harvard repository of research data. It is published by MIT's Election Data and Science Lab. We converted the dataset to a CSV and cleaned it in this notebook by dropping irrelevant columns.

### Geopy
We used Geopy to add latitude and longitude as columns to our 'hc_by_agency' dataset so we could create a heat map to better visualize hate crimes by area in the United States. We did this by importing geopy into the notebook and pulling requests from Google Maps by passing in a string of the city name and state to the geopy method. This returned a pair of coordinates which we then added to the hc_by_agency data frame. We saved this data frame as a CSV because the geopy package takes a very long time to run and can only handle 4000 requests at a time.

# Description of Data and Descriptive Statistics

Once we had all our data in order, we began grouping, plotting and finding the descriptive statistics from the data in order to both better understand it and inform our decisions during the modeling process. We will discuss the steps, findings, motivations and descriptions of our exploratory data analysis below.

!['hc_by_bias'](img1.png)

!['hc_by_agency'](img2.png)

!['elections_head'](img3.png)

### Dataset descriptions

- 'hc_by_bias' dataframe contains offenses by bias motivation per year. These statistics come from Table 1 ("Incidents, Offenses, Victims, and Known Offenders by Bias Motivation") of the UCR datasets from 2004-2019. Each row indicates the type of motivation for the hate crime and the total count of such crimes for that year.
- 'hc_by_agency_sample' contains the name of the reporting agencies with their latitude and longitude coordinates. This is a sample of nearly 4000 locations from the main hate crime dataset 'hc_by_agency'. 'hc_by_agency' is the result of the combined data from the UCR Program's Hate Crime Statistics from 2004-2019.
- 'elections' dataframe contains information from all U.S. Presidential elections from 2000 to 2020. This includes the candidates' party affiliation and how many votes they received per state. For example, Bush, running for the Republican party, received 941,173 votes from Alabama in 2000.

There are some important things to be cautious about with the hate crime data -- many of the values recorded as NaN are not necessarily 0, but rather unreported. Many of the more rural/conservative/less populated areas do not report their total hate crimes and may be reporting incorrectly (ex. things that are legally considered a hate crime are not counted as such in the agency's records). With this in mind, we must fill in the NaN values with 0 for the sake of manipulating data but also must recognize that the recorded number of hate crimes in certain areas may be off due to intentional misreportings/misclassifications.

The 'Quarters reported' column in 'hc_by_agency' represents the number of quarters of the year that each agency reported hate crimes. When the value is NaN, it means the original dataset counted it as 0 and, as explained in on the FBI website, does not necessarily reflect the truth.

Overall, we must take this data with a grain of salt; this is the most comprehensive data available on hate crimes, but it is not necessarily wholly accurate due to biases/misclassifications/misreportings in many of the areas in the United States.

### Data relevancy

- The 'hc_by_bias' dataset contains hate crime incident counts as reported by agencies across the United States from 2004-2019. This information will help us gauge the overall trend of hate crimes regarding offender and victim demographics by year. The hate crimes reported in this dataset are archived based on the following bias motivations: Race/Ethnicity/Ancestry, Religion, Sexual Orientation, Disability, Gender, and Gender Identity.  We will be able to calculate the total amount of incidents for all six different types of bias motivations across 15 years, which will inform our analysis on how hate crimes have changed over time.
- The rows in the 'hc_by_agency' dataset correspond to an agency that reported at least 1 hate crime incident that year. The number of incidents per agency is broken down by bias motivation. 'hc_by_agency' also includes the population of the reporting agency (if that agency operates at a city level), which will be important to get a more accurate perspective of the true impact of hate crime relative to the population. The 'hc_by_agency_sample' is a subset of 'hc_by_agency' that contains the latitude and longitude of the city where an agency is located.
- The 'elections' dataset contains information on the presidential elections for each state and the number of votes each candidate received according to their party affiliation. This information will be used to build a supplementary analysis regarding a potential correlation between political party and types of hate crime. This dataset contains other features such as the votes the candidate received from each state, which indicates the general political atmosphere and can thus be used as another feature when looking at a correlation/prediction model.
- These three datasets will allow us to cross-analyze for socio-political atmosphere. Furthermore, we will look into a potential correlation between major events and the quantity and frequency of hate crimes.

### Hate Crimes by Bias (2004 - 2019)

#### Average number of incidents, offenses, victims, known offenders each year per bias motivation
The table below shows the 10 biases with the highest average number of incidents. For each year from 2004 to 2019, there were about 2,384 anti-Black hate crimes.

!['avg by bias'](img4.png)

#### Standard deviation of incidents, offenses, victims, known offenders each year per bias motivation
The table below only shows the 10 biases with the highest total incident standard deviation. For each year from 2004 to 2019, the amount of anti-Black hate crimes deviated by about 370 incidents on average.

!['sd_by_bias'](img5.png)

### Hate Crimes by Agency (2004-2019)

!['hc_by_agency'](img6.png)

The average number of hate crimes reported by an agency is 3.5517776428523242 incidents. The standard deviation of total hate crimes reported by an agency is 11.945990817869317 incidents.

The table below shows the average number of incidents reported by a single agency for each bias motivation by year. It also includes the average total number of incidents reported by a single agency that year (last column). Incidents motivated by race/ethnicity/ancestry bias consistently have the highest mean from 2004 to 2019.

Note that Gender and Gender Identity were introduced as categories in 2013, so any average values for that column are listed as NaN.

!['hc_by_agency'](img7.png)

The table below shows the standard deviation of number of incidents reported by a single agency for each bias motivation by year. It also includes the standard deviation of total number of incidents reported by a single agency that year (last column).

Again, note that Gender and Gender Identity were introduced as categories in 2013, so any average values for that column are listed as NaN.

!['hc_by_agency_year'](img8.png)

The table below shows the maximum number of total incidents reported by a single agency that year. Interestingly, the maximum number of recorded is much higher than the mean. This reflects our understanding that some agencies report more accurate totals than others.

![''](img9.png)

### The following cells reflects incident counts from all agencies
The data used above and in the rest of the notebook excludes data reported by federal and college agencies, as well as data from the U.S. territories.

![''](img10.png)

While the mean total number of reported incidents per agency remains relatively consistent over the 16 year period, the standard deviation increases steadly starting in 2014. The minimum number of recorded incidents is 1, which aligns with the fact that only agencies that report **at least** one incident are included in this dataset. Agencies that did not report any hate crimes that year were recorded in a separate dataset each year.

### Hate Crimes by Agency (Sample)
Note the latitude and longitude columns. See the 'Data Cleaning' section to see how this sample was chosen and how the latitude and longitude columns were added.

!['hc_by_agency'](img2.png)

### Presidential Election Results (2000-2020)

!['election'](img11.png)

# Data Visualizations

#### Explanation of Visualizations, Feature Selection and Extended Data Cleaning Procedures

We did a myriad of data cleaning procedures and feature selection—we first did a series of graphing to better visualize and understand the data. We used heat maps and bar plots to help us move forward with our next steps in the data cleaning process (i.e. highlight discrepancies, patterns, etc.). We had to go through a process of cleaning the data within the EDA portion that was subsequently extended into the modeling portion where we converted strings to ints, label-encoded categorical data (such as city names, agency types, political party affiliation) and made sure that all of our data was plottable. After this, we cleaned the Elections dataframe, which originally had the number of votes for every candidate in each state for a given year. We cleaned this data so that we could isolate the winner in each state and their respective party. We then joined the hate crime data frame with the elections data frame so that we had the party affiliation of a state in a given year—thus, we were able to use a state’s party affiliation as a feature.

### Heat map of hate crime locations in the United States (2004-2019)

The heat map below shows the location of the agencies included in our random sample. Note the concentration of agencies on the coasts and in the South. In particular, notice how the distribution of reporting agencies in California also aligns with the state's population distribution (the Bay Area and Southern California are both red).

!['heatmap1'](img12.png)

!['heatmap2'](img13.png)

!['heatmap3'](img14.png)

## State

Immediately, California stands out as the state with the most incidents. We need to be cautious of this because California may just be doing more reporting of incidents, given that 86.1% of participating law enforcement agencies reported that no hate crimes occurred in their jurisdiction as recent as 2019.


```python

```


    
![png](output_48_0.png)
    


## Bias motivation

The following plot only includes the states with the highest number of reported hate crimes. According to the raw count, California has the most "Race/Ethnicity/Ancestry" bias-motivated incidents than any other state. New York has the most religion-biased hate crimes than any other state. Race/Ethnicity/Ancestry bias-motivated hate crimes appear to be the most common.


```python

```


    
![png](output_51_0.png)
    


## Agency type

The first bar plot is a count of the total incidents per agency type. The results demonstrate that there are more reporting city agencies than other types of agencies (ex. county, state police agencies).

The second bar plot is the count of incidents per bias motivation by type of agency. Similar to the plot above, cities report the most hate crime incidents for nearly all types of bias motivations.

NOTE: Many of the nonmetropolitian areas are 1) more rural 2) more conservative and 3) report less hate crime statistics, so even though it looks like they have much lower numbers, it is more than possible that they have a proportional amount to cities/metropolitian areas given their respective populations


```python

```


    
![png](output_55_0.png)
    



    
![png](output_55_1.png)
    


## Time

This plot depicts the change in different types of hate crimes between 2004 and 2019. The top purple line represents the number of Race/Ethnicity/Ancestry bias-motivated incidents, which had been decreasing since 2004 but began to increase after 2014.

The Gender and Gender Identity biases were introduced as a category in 2013. The Ethnicity/National Origin bias was combined with the Race bias into the "Race/Ethnicity/Ancestry" category in 2015. 


```python

```


    
![png](output_58_0.png)
    


# Modeling 

After our exploratory data analysis, we moved into modeling. For this portion, we began with label encoding as we needed to turn categorical and descriptive data into numerical data so it can be plotted and interpreted by the computer. We used a series of correlational plots, the graphs above, and descriptive statistics to decide which features to use. We also tested several different types of models and used different methods of modeling (decision tree, OLS, Ridge, LASSO) to best predict hate crimes and better understand our data as a whole. Though the models did not perform perfectly, the process itself revealed a lot about what we needed to do in order to pursue further research and better the model.

### Data reorganization: Creating the political party affiliation feature
To prepare the data for modeling, we had to add a column to the hc_by_agency dataframe that would indicate whether the president for each year was Republican or Democrat. The election dataframe provided data on which party won the presidential election for each year. However, this data only records election winners every four years (since each term is 4 years). This is a problem because hc_by_agency records data on a yearly basis. 

To correct this difference, a for loop was used to create duplicate rows for each four year period within the elections dataframe. For example, the data for the row corresponding to the year 2000 would be copied and three more rows would be created with the identical information for years 2001, 2002, and 2003.  Additionally, since each hc_by_agency has a granularity on the city/state level, a merge of the two dataframes was done with the year and state columns.  This works since both data frames have these two columns in common.  The merge results in a new dataframe, combined, which now displays which party won the presidential election during each time period for each city of each state. 

### Feature Selection
Along with party affiliation, we used location, agency type (city, metropolitan area, non-metropolitan area, etc.), year, number of quarters reported (by the state / agency) and population as features to predict the number of hate crimes reported by an agency. As we are predicting the total number of hate crime incidents we had to drop the subsections of hate crime instances (number of reported hate crimes motivated by biases based on religion, sexual orientation, gender identity, etc.) from our data when creating the ‘X’ data to be plotted. 

We also created a correlation matrix to visualize the correlation between all features, which allows us to pick the ones with correlation values higher than a certain threshold. After doing these procedures, we were able to move on to the actual modeling portion of our project.


```python

```


    
![png](output_63_0.png)
    


### Explanation of Modeling Process

As our goal is to predict hate crimes given the history of them, we chose to use OLS, or Linear Regression, as our model. Linear regression is an appropriate choice because we want to predict one variable (total number of hate crimes) given other variables (features) and the two types of variables (independent and dependent) have a relationship with one another. Furthermore, since OLS is a regression model, it helps to prevent overfitting and is easily interpretable, implementable and overall simple, making it an appropriate choice for our modeling portion. OLS, or Linear Regression, is a type of regression that attempts to model the relationship between two variables by fitting a linear equation to observed data by calculating the best-fit line for observed data by minimizing the sum of the squares of the vertical distance from each data point to the best-fit line. We tested both LASSO and Ridge Regression models (given that these are also foundationally appropriate choices for supervised learning, regression problems) and found that the OLS Linear Regression model provided us with the lowest RMSE and best r^2 score—thus, we decided to move forward with OLS model.

We also decided to remove the outliers (values bigger than 80) and create, train, validate and test a new model with only these values. We did this not only to compare the two models, but because we thought it was necessary to understand how outliers affect data prediction.

Furthermore, given that this is a regression centered, supervised learning problem and our data is entirely composed of discrete values, we also wanted to explore non-parametric options such as decision trees. Thus, we implemented a decision tree approach and created a model that predicts the value of the y-variable by learning from the data features (X). Decision trees can help solve regression problems by splitting data into various decision nodes and following the path that produces the best results in order to better the model it is creating. This approach improved our model, so much so that we worried that we had overfit the data. We wanted to include both the decision tree model and the OLS model so as to provide a multi-dimensional view of our data models and its ability to predict hate crime incidents. We also cross validated our model.

### Train Linear Regression Model


```python

```


    
![png](output_67_0.png)
    


The RMSE on the training set is 7.915503887378856. The training model's R^2 value is 0.5700816333390107. The training model's mean absolute error is 2.5563270397795708.

See "Explanation of Modeling Results" section for our interpretation of these values.

### Train LASSO Regression Model


```python

```


    
![png](output_70_0.png)
    


The RMSE on the training set is 7.9155151378709405. The training model's R^2 value is 0.5700804112319442. The training model's mean absolute error is 2.5535520882839537.

See "Explanation of Modeling Results" section for our interpretation of these values.

### Train Ridge Regression Model


```python

```


    
![png](output_73_0.png)
    


The RMSE on the training set is 7.915503887993281. The training model's R^2 value is 0.5700816332722678. The training model's mean absolute error is 2.5563068010194976.

See "Explanation of Modeling Results" section for our interpretation of these values.

### Train Decision Tree Model 


```python

```


    
![png](output_76_0.png)
    


The RMSE of this model is 0.005301925850180039. The tree model's R^2 value is 0.9999998071160171. See "Explanation of Modeling Results" section for our interpretation of these values.

### Test Linear Regression Model


```python

```


    
![png](output_79_0.png)
    


The Linear Regression Model RMSE on the test set is 8.084348621492806. The Linear Regression Model Test set r^2 score is 0.5859265657181845. The Linear Regression Model Test set mean absolute error is 2.6177753920531215.

**Best predictor according to OLS model**

Our best predictor is state political party, as indicated by the fact that party has a coefficient of about -0.8428. According to our OLS model, agencies in states where a Republican presidential candidate won reported about 0.8428 less hate crime incidents on average.

### OLS Model Cross Validation

The Linear Regression Model Cross Validation r^2 score is 0.534539330692931. The Linear Regression Model Cross Validation rmse is 8.149976315277929. See "Explanation of Modeling Results" section for our interpretation of these values.

### Test Decision Tree Model


```python

```


    
![png](output_86_0.png)
    


The Tree Decision Model Test RMSE is 6.364464478213. The Tree Decision Model Test r^2 score is 0.7433678658732965. See "Explanation of Modeling Results" section for our interpretation of these values.

**Best predictor according to decision tree model**

The best predictor is population, as indicated by the fact that population has a coefficient of about 0.6403. According to our decision tree model, agencies in states with a higher population reported more hate crime incidents on average.

### OLS, Decision Tree Models (Without outliers)
See explanation for repeating the modeling process without outliers in previous "Explanation of Modeling Process" section.


```python

```


    
![png](output_91_0.png)
    


We also graphed the above plot with logarithmic axis so we can better see data that was grouped together near the origin.


```python

```


    
![png](output_93_0.png)
    


The Linear Regression without Outliers Train RMSE is 4.561884936877587. The Linear Regression without Outliers Train r^2 score is 0.2165248711702733. See "Explanation of Modeling Results" section for our interpretation of these values.

#### Test Linear Regression Model Without Outliers


```python

```


    
![png](output_96_0.png)
    


The Linear Test set without outliers rmse:  4.8256817614025165. The Linear Test set without outliers r^2 score: 0.23724802900795106. The Linear Test set without outliers mean absolute error: 2.1963228180432717. See "Explanation of Modeling Results" section for our interpretation of these values.

#### Train Tree Model (Without Outliers)


```python

```


    
![png](output_99_0.png)
    


The RMSE of this model is 0.005301925850180039. The tree model's R^2 value is 0.9999998071160171. See "Explanation of Modeling Results" section for our interpretation of these values.

#### Test Decision Tree Model (Without Outliers)

The Tree Test Set RMSE:  4.8938659432752605. The Tree Test Set r^2 score: 0.21554123565703387. The Tree Test set mean absolute error: 2.1407545254610048. See "Explanation of Modeling Results" section for our interpretation of these values.

### Explanation of Modeling Results

With our OLS model with the outliers included, we got a RMSE of 8.08, an R^2 score of 0.58 and a mean absolute error of 2.62. With the outliers removed, our RMSE improved marginally to 4.82, our R^2 score became 0.237 and the mean absolute error was 2.19. With that, the model without outliers was marginally better, which was to be expected. We also showed the model with a logarithmic axis to better understand the data as the data had many growing values and was difficult to interpret when all of the points were clustered towards the origin.

As the square root of the variance of the residuals, the RMSE is a measure of the difference between our predicted values and the observed number of hate crime incidents recorded by an agency that year. Relative to our LASSO and ridge models, which respectively had RMSEs of about 8.3475 and 8.3474 when outliers were included, our OLS model performed better (as indicated by its lower RMSE).

The $R^2$ value of the models we chose represents the percentage of the variation in incidents reported by an agency that our features explain. Our LASSO and ridge models both had $R^2$ values that were less than 0.58. Since we were more interested in prediction rather than measuring the relationship between our features and the number of reported incidents, we placed less value on the $R^2$ value and more on the RMSE when deciding which model was best.

Graphing the OLS model revealed clustering towards the origin of the graph, showing us that the model was not very accurate when predicting low values and/or that the model has a lot of zero values. This is partly because of the incompleteness of the original data from the UCR database and is a reflection of the government’s inability to collect data on hate crimes in different areas, particularly non-metropolitan areas.

The decision tree model ultimately performed slightly better than the OLS model when outliers were included, with a RMSE of 7.12, an R^2 score of 0.68 and a mean absolute error of 2.33. However, when we removed outliers, the decision tree model actually performed worse than the OLS model with a RMSE of 4.89, an R^2 score of 0.22 and a mean absolute error of 2.14. While the decision tree model performed extremely well on the training data, it performed about as well as the linear models on the test data. This is potentially due to overfitting, a common concern with decision trees. Because we intended to create a model with forecasting capabilities, we decided to use our model to predict both the total hate crime incidents an agency would report and how many incidents motivated by a specific kind of bias an agency would report. To predict the total number of incidents an agency would report in future years (any year after 2019), we chose to isolate a random agency—Mayville Police Department in Wisconsin—and fill in information from 2020 for each of the features. After doing so, we used our OLS model to see how many hate crimes it predicted would be reported by that agency. In order to predict bias-specific counts of incidents, we used all of the features, including ‘Total incidents,’ to predict how many race/ethnicity/ancestry bias-motivated hate crimes an agency would report in a given year. This attempt at prediction was not as successful as our initial modeling attempts, which indicates that our model might not be the best suited for forecasting hate crimes.

We used several different types of models and several slightly different iterations of the models in order to see which model would perform the best as well as to identify areas of improvement. Our model without outliers resulted in a lower RMSE, which makes sense since we removed data points that would have confused and/or not been helpful to the predictive process. Our decision tree model worried us about overfitting the data, but we kept it in for the purpose of understanding the modeling process and because we felt it was a good predictor method. We also have a lot of zero entries because of gaps in the data which led a lot of our data to be clustered around the origin--we will explain the reasons for and implications of these data gaps in the conclusion. 

### Forecasting total hate crimes reported by an agency

Here, we decided to try and predict a given city's hate crime. We chose a random row and used our model to predict the total number of hate crimes in that area given the features associated with that city. Our model predicted the total number of hate crimes to be 1.69 and the actual answer was 2. 

#### Predict Hate Crimes in a Specific City using the Model Above

We randomly chose the city of Mayville, Wisconsin to try and predict how many hate crimes it would report in 2020.

!['mayville'](img15.png)

We created a row for Mayville in 2020. All features were kept the same except population (we plugged in the population of Mayville in 2020) and party label (WI went from Republican in 2016 to Democrat in 2020).

!['mayville_2020'](img16.png)

The predicted number of hate crimes reported by the city of Mayville, WI in 2020 is: 1.5363634327803197.
The actual number of reported hate crimes by the city of Mayville, WI in 2020 is 2.

# Conclusions

Our initial goal with this project was to investigate how hate crimes have changed over time and create a model to predict the total number of hate crimes given the data collected by the FBI. We also sought to find out where the most hate crimes occur as well as which groups of people these crimes are most often committed against. With this project, we were able to accomplish our goals, though to varying degrees. We found that the most amount of hate crimes are committed against people with the bias motivation of Race/Ancestry/Ethnicity--this was the same across almost all states. We also found that most hate crimes take place in metropolitan areas and cities. We were also able to conclude that though hate crimes have changed a bit over time, the aggregate data (i.e. sum total) and trends have stayed relatively the same over the past 15 years. We were even able to reach our goal of predicting hate crimes using a Linear Regression model and machine learning, though this model definitely has issues and downfalls that will be discussed further. Another potential goal we had upon the conception of this project, though it had to be modified over time due to the incompleteness of the data (discussed below), was to draw potential associations between worldly happenings / events and influxes in hate crimes. For instance, we posited (in a very general sense) that hate crimes against people of Muslim religion might have gone up within the months after the attacks on 9/11--due to inconsistencies and lack of data, we were unable to prove or disprove this hypothesis, another gap that will be discussed below. 

After analyzing our data, we can see that though predicting hate crimes to some extent is possible, it is a very complicated and impacted process. Similar to the tendencies of and problems surrounding predictive policing, predicting hate crimes is a very tricky and sensitive operation. Understanding and interpreting trends within the realm of human actions, especially those impacted by anger and/or emotions (such as hate crimes) is a very difficult process. Though some features are helpful in predicting these actions, as seen in the relative success of our model, the overall process is still very difficult to trace and forecast. Our predictive methods served our goal of predicting hate crimes as we were able to successfully implement our model and predict hate crimes in Mayville, Wisconsin--whereas the actual number of total incidents was 2, our model predicted 1.7. Though this model did not perform exceptionally well, this is largely due to gaps within the UCR data itself. If we were to do this project again, we would use more data from other areas of research as cross-references / additions to the UCR data. As discussed below, the UCR data has many inconsistencies and gaps that necessitates supplementary data from elsewhere. We attempted to solve this issue through the inclusion of the ‘elections’ data frame, in which we added the party affiliation of a given state in a given year to use as another feature to predict hate crimes. We also attempted to solve this issue of data gaps with our use of geopy, where we added a precise location to a given area so we were both able to visualize hate crimes on a heat map as well as use geographic location (coordinates) as another feature to predict hate crimes. We also removed outliers from our data and created a new model with no outliers to try to solve this issue of data inconsistencies. Though these remedies worked in some respects and bettered our model’s predictive abilities, they did not solve the issue of data gaps altogether. Thus, we will now move into a discussion of the issues and gaps within the data and our recommendations for future research. 

To begin, a large portion of the FBI data did not have actual value--many non-metropolitan areas either refused to or did not submit data to the UCR data base and, as a result, those areas were essentially nullified. However, this does not mean these areas were devoid of hate crimes, or even have lower rates of hate crimes. According to further research regarding hate crimes conducted by the National Institute of Justice, the government cannot legitimately enforce that hate crime data be collected and / or submitted (Shively, Understanding Trends in Hate Crimes, 31). As a result, only one third of law enforcement agencies in the United States actually report hate crime statistics (Shively, Understanding Trends in Hate Crimes, 31). This has led to a large gap in data regarding hate crimes. Of course, with more access to relevant data and features, the more accurate the model. If someone were to build such a model over again, they would benefit from having more data, perhaps from privately acquired research or even more private government statistics. Since our group only had access to public data that was largely incomplete due to lack of government enforcement, it was difficult to create an extremely accurate model. 

This leads into a major policy issue: as the government cannot enforce the collection and / or submission of data regarding hate crimes, it is nearly impossible to fix the issue as a whole. The inability to correctly identify which areas have legitimate issues with bigotry and bias has essentially stunted the nation’s capacity to address the issue of hate crimes. However, to take a top down approach and enforce the collection of hate crime statistics would be an entirely different policy issue surrounding state and federal rights / powers. Thus, as mentioned above, private data collection might be the solution to this issue--but this implementation would create separate issues in and of itself. Paying private companies to collect data on hate crimes would create a series of legal and ethical implications such as the motivation behind data collection and the fact that companies would be paid to collect / analyze this data (potential biases and ulterior motives) as well as the fact that definitions of, training to classify and policies surrounding hate crimes change both over time and from place to place (Shively, Understanding Trends in Hate Crimes, XVI). With this in mind, categorizing and aggregating data on hate crimes becomes a borderline subjective task and, without federal regulation and standardized rules (such as those implemented by the FBI), legal and ethical implications have the potential to ensue. Thus, it is very important that the collection of data surrounding hate crimes is, at the very least, overseen by federal entities that can regulate the process across the board. 
Another legal and ethical issue here is that many of the hate crimes are committed with biases such as race/ethnicity/ancestry, gender identity, disability and sexual orientation. With this in mind, the collection of hate crime statistics is also very sensitive and has ethical implications because many of the victims are illegal immigrants; many are not open about their gender identity or sexual orientation; many have disabilities that may not be widely known to employers or other private entities. Thus, to collect data on these victims is to impede on their privacy and force them to disclose information, potentially putting them in further danger. Thus, through the forceful collection of hate crime data would no doubt help the issue of lack of data and poor model creation, it would also have major ethical implications for the victims of hate crimes (the very people this sort of data collection is supposed to help). This also raises a legal implication of putting minority populations (immigrants, LGBTQ+ identifying individuals, people struggling with gender identity, people with disabilities) in a potentially even more compromising position as well as just a blatant invasion of privacy. Collecting data is a difficult process since there is a fine line between asking too much and not asking enough.

The current features within the hate crime data include locations (though not specific locations, such as coordinates, as we had to add those manually), population, quarters reported, year, type of agency and, with our combining of data, party affiliation and geographic location. Agency type proved to be a particularly helpful predictive feature in particular. Party affiliation was also a good predictive feature but, as many of the states did not submit or submitted incomplete data regarding hate crimes, it is difficult to call any single feature particularly effective at predicting hate crimes as a whole. Thus, this is a gap we identified in our research and a potential place for researchers in the future to improve upon. We recommend using features such as geographic location, agency type and party affiliation as these proved to be decent predictive features. We also recommend that future researchers look into historical events surrounding hate crimes which can be further explored if individual instances of hate crimes are better recorded. With more precise and detailed data, future researchers might be able to draw connections between worldly events and an influx in hate crimes against a certain group of people. As the FBI UCR data did not include specific dates, or even more general months, it was very difficult to make any kind of assumptions on hate crimes against certain groups and a potential correlation with worldly happenings. Though we were able to draw some hypothetical conclusions, as mentioned above, we suggest that future researchers attempt to gain access to more detailed data that might provide better insight into why changes happen over time.

Furthermore, as data such as the UCR FBI data is used to design policies and influence policymakers, it is important that this data is accurate and all-encompassing. However, this is not currently the case. Thus, we now move into recommendations for future areas of research. As mentioned above, it is important for the process to be overseen by a regulatory entity such as the federal government. With that, we recommend that the government work directly with individual states to create policies that will implement and enforce the collection of hate crime statistics. We also recommend that the UCR database release more information about the individual instances of hate crimes as this will provide researchers with many more features and data entities with which to predict hate crimes. Features such as specific dates, locations, qualities about the neighborhood / area in which the crime took place, what qualified the crime as a hate crime and even qualities about the victim and/or criminal committing the crime will all aid in future research not only for predictive reasons, but also for policy recommendations. Legal issues and ethical issues are no doubt tied to these recommendations. However, adjustments such as these may lead to more extensive research and data collection on hate crimes as a whole and, as mentioned above, the issue of hate crimes will not be fixed until more attention is paid to it and more resources are put into stopping them. With that in mind, we hope that this project has illuminated an area of research that is desperate for more resource investment and attention.