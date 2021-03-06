---
layout: page
title: Group 03
nav_order: 8
description:
---

# Predicting Police Killings Within Counties in the US Through Walmart Frequencies and Demographics

Group Members: Pardis Bidokhti, Edward Chang, Alisha Lewis

## Research Question:

With the recent headlining police killings of George Floyd, our group chose to analyze what factors affect police violence. Our country is reckoning with the institutionalized racism in our police system and we must consider what aspects make areas more susceptible to police brutality, in this case, the number of Walmarts in counties. 

There is a general attitude that Walmart’s primary consumer base is composed of low-income shoppers: a 2019 Business Insider study showed that the average American Walmart shopper has an annual household income of just over \\$76,000. Alternatively, the average Amazon consumer has an annual household income of \\$84,000. While studies have been conducted comparing customer satisfaction in low-income neighborhood Walmarts with those in more affluent neighborhoods, as well as studies about Walmart’s primary consumer base, the volume of Walmart stores in low-income areas is unclear. Considering that policing occurs more frequently in low-income, minority neighborhoods and Walmarts serve low-income families, we were curious to see if there is an associative relationship between the number of Walmarts in neighborhoods and the amount of police violence, specifically in California where there is a large income gap. 

When we first started this project we only used data from California, but found that we did not have enough information to make a proper prediction. As police violence is prevalent throughout the United States, we thought it would be important to consider counties across the country. Our datasets were grouped by county to analyze which areas had factors that led to differences in police killings. We considered factors such as race and poverty levels, along with the number of Walmarts in a county. As such, our research question is the following: How are the frequency of Walmarts and the profile of a community related to the amount of police killings in the United States in 2019? More specifically, are the profile of a community and frequency of Walmarts in counties good predictors of the number of police killings in the US?

## Finding and Assembling Data:

To find our datasets, we chose reliable, public data. For information about the community profile, we accessed data from the American Community Survey conducted by the United States Census Bureau. From this dataset, we gathered information about race and poverty levels of each county. To look at data about police violence, we used information collected by mappingpoliceviolence.org. This organization collects data regarding police killings across the United States and categorizes by race, armed status, and a variety of other factors. Lastly, we looked at the location of Walmarts across the US from a Kaggle dataset and aimed to see if this affected the amount of police killings. Using these three datasets, we separated our data into training, test, and validation sets to attempt to predict police killings. 

### Description of Three Datasets:

- [American Community Survey (ACS)](https://data.census.gov/cedsci/) is from the online database of ACS data provided by the American Census Bureau. It provides information about the United State’s population and economy, but we chose a couple specific tables to focus on. We accessed data about the percent of population below the poverty line per county. Additionally, we used data about  the racial makeup of each county. We believed that race and socioeconomic status were the most important factors when considering police brutality. This dataset gives us an overview of the community profile of each county.
- [Mapping Police Violence](https://mappingpoliceviolence.org/) is from an organization that aims to collect data on police violence to illustrate the impact of police brutality on certain communities. Along with the name of the victim, this dataset provides information on a description of the situation, police department responsible, armed status, body camera footage, county, criminal charges, date of event, victim’s information, and other important data. Each row in the dataset gives information on an individual police killing. As we are only looking at 2019 data, we isolated the number of police killings in each county across the US in that year. This information along with the ACS data allowed us to consider what community profiles are more susceptible to police violence. 
- [Walmart Locations](https://walmart-open-data-walmarttech.opendata.arcgis.com/) is a public and reliable database. There is information about the longitude and latitude, address, state, county, city, store number, and open status of each Walmart location. Using this information, we were able to sort this data into how many Walmart stores there were in each county. When combined with our other data, we were able to consider if Walmart stores were also a factor in predicting police violence. 

### Data Cleaning:

We merged these datasets together on the county as that is shared between each dataset. We chose to keep the poverty rate, race, number of Walmarts, and killings of each county as the data we considered. From this data, we created a model to predict where police violence would occur in the future. After merging the ACS dataset of all US counties as well as the number of walmarts per person, we ended up with 320 data points, which still would not be sufficient for a predictive model. As such, the low number of counties was a hindrance that we could not avoid since the county column was the only column that we could merge the Walmart and ACS datasets on. It's important to note that to account for null values during the data cleaning process, we dropped any counties with null values in order to preserve data accuracy. Moreover, to standardize our data, we made each instance a percentage of the total population per county.

Our final data set has the following variables:

| Variable             | Description                                                                        |
|----------------------|------------------------------------------------------------------------------------|
| `county`             | The name of the county                                                             |
| `percent_poverty`    | The percentage of people identified to be as in poverty                            |
| `white`              | The percentage of people who identify as white                                     |
| `black`              | The percentage of people who identity as black or African American                 |
| `native`             | The percentage of people who identify as American Indian or Alaska Native          |
| `asian`              | The percentage of people who identify as Asian                                     |
| `pacific_islander`   | The percentage of people who identify as Native Hawaiian or Other Pacific Islander |
| `other`              | The percentage of people who identify as an other race                             |
| `two_mixed`          | The percentage of people who identify as mixed race                                |
| `walmart_per_person` | The number of Walmarts per person                                                  |
| `kill_per_person`    | The number of police related killings per person                                   |

There are some problems with our data set, for example:

A caveat in the data was missing county information in the American Community Survey (ACS). Our goal with the ACS dataset was to obtain the demographics associated with every county, including sex ratio, poverty level, and race. Because the some of the data was missing counties, we omitted those counties during the final merge. As such, our final data excludes the demographics associated with the unknown counties and is not entirely comprehensive of the united state's population.

In the process of merging the ACS dataset of all US counties as well as the number of walmarts per person, we ended up with 320 datapoints, which still would not be sufficient for a predictive model. As such, the low number of counties was a hindrance that we could not avoid since the county column was the only column that we could merge the Walmart and ACS datasets on. It's important to note that to account for null values during the data cleaning process, we dropped any counties with null values in order to preserve data accuracy. Moreover, to standardize our data, we made each instance is a percentage of the total population per county.

## Feature Selection Analysis:

Due to the nature of our data, the following visualizations were the only visualizations which could give meaningful insights towards our data and feature selection.

For features selection, we referred back to the heatmap below, which demonstrated the relatedness of the features we chose for our model. Because we all have domain knowledge related to this topic, through this class and otherwise, we know that police killings disproportionately target people of color and minority communities. As such, we found race and poverty levels to be sufficient indicators of police killing likelihood. Moreover, since this project seeks to establish causality between the number of police killings and number of Walmarts per county, we found the number of Walmarts per person to be a feasible feature as well. Again, we chose Walmarts because of its history of serving primarily low-income shoppers with its low prices.

![Heat map of our variables](heatmap.png)

Our correlation heatmap below shows similar findings compared to the clustered heatmap of the variables. Here, it is more clear that we see little demographic information having a straight relationship between the walmarts per person and the kills per person. Interestingly enough, we see larger percentage of natives correlated moderately postively with the kills per person in both heatmaps while more asian percentage is correlated moderately negatively. On the walmart per person side, we see that median age is negatively correlated.

![Correlation heat map of our variables](corr_heatmap.png)

## Model Selection:

We chose a predictive model rather than a clustering model because we wanted to see how well the algorithm could predict the amount of police killings based on features that are believed to affect police violence, such as race and poverty levels, as well as number of walmarts (which our EDA showed there's some correlation between number of walmarts per person and police killings). A clustering model would group together similar data points based on similarities and identify clusters, which would not serve the purpose of what our question is trying to achieve. A predictive model would instead suggest if the selected features are good predictors of police killings. 

We chose to use Ordinary Least Squares (OLS), Ridge Regression, and Least Absolute Shrinkage and Selection Operator (LASSO) to train our model. Using these models also helped us to consider which features were the most important for predicting where police violence would occur. 

OLS uses the principle of least squares, minimizing the sum of the squares of the differences between the observed dependent variable and the predicted. As it is a fairly simple calculation and can be converted to code easily, it is commonly used. The flaws of OLS are that it does not handle outliers well, many real life situations cannot be described linearly, and it can't handle a large number of features. OLS could be useful for this model as there are not many features, it is unbiased, and has low variance.

Additionally, we decided to run a ridge regression as we thought some of our variables may be correlated and result in multicollinearity. Ridge regression can prove to be better than OLS as it deals with overfitting due to the number of predictors exceeding the number of observations. Ridge deals with this by adding bias to the regression estimates. It does not need unbiased estimators as it adds enough bias. Ridge can pose problems as the regularization doesn’t result in elimination of coefficients or sparse models, which can result in more complicated models with lots of features. We still thought this would be a good technique as there is a bit of multicollinearity. 

As another model, we chose to do Lasso regression. Lasso regression is another variant of linear regression that uses shrinkage to encourage a model with fewer parameters. It uses L1 Regularization, which eliminates sparse models so it results in a simple model. Lasso uses feature selection so it is good to use when there are lots of variables that we need to eliminate. The main problem with lasso regression is that with correlated variables, it retains only one variable and sets other correlated variables to zero. That will possibly lead to some loss of information resulting in lower accuracy in the model. Since our model already has only a few features and some seem to be correlated, this model might not be the best option. 

### Initial Steps in Model Development:

![Pie chart breakdown of full set split](piechart.png)

We set 20% of the data for testing, 20% of the data for validation, and 60% for training. We use 60% of the data for training so that we have amble data to make an accurate model. We have to reserve 40% of the data for testing and validation so that we have enough data to see if the model is accurate. If we used more data to train, there would be more for the computer to learn, but we wouldn't have enough data left over to make sure that the model is accurate with testing and validation. If we used less data to train, the model would probably be less accurate as there aren't as many points for it to learn off of. For our X features, we dropped the county and kills per person as that would create a biased model and potentially cause overfitting. For Y, we selected the kills per person as this is what we are trying to predict. 

Following that, we used OLS, Ridge Regression, and LASSO to train our models on this data. 

### Modeling Results:

![Linear modeling results](linearresults.png)
![Ridge modeling results](ridgeresults.png)
![Lasso modeling results](lassoresults.png)

For model selection, we tried linear regression, ridge regression, and LASSO to see which model resulted in the highest $R^2$ score. The results of this can can be seen in the output above; the data points in linear regression are the closest fitted to the regression line, and the LASSO regression needs to have run into a problem regardless of the hyperparameters provided to it--resulting in a $R^2$ of 0 and it is a constant predictor, meaning that the LASSO model does not explain any of the variability of the police killings data around its mean. Broadly speaking, these models aren't very good predictors since even the non-zero r2 scores are around or below 0.5, meaning that only about half or under half of the variance in the number of police killings is explained by the model.

These poor predictors were more than likely a result of a lack of data points, since we only had 320 total data points. It could also be a result of a lack of features, however, adding more features likely would not change the accuracy of the model by much, since there was not enough data to train the model on. There is also an endogeneity problem in that there could be external variables not included in the model that are related to both police killings and the features used in this model.

### K-Fold Cross Validation Results: 

We performed 5-fold cross validation to see how well our models would predict on unseen data within the training set. We chose 5 folds because the dataset was small and we felt that 5 would be sufficient to be statistically representative of the data. We got different results than when just finding $R^2$ on the training data. Our LASSO model is still the worst model, and the ridge model seems to show evidence that it is performing better than our linear model now. In this part, we also see that the constant predictor nature of the LASSO is not unique to just the full model, and that the same time is happening within each subset of our training data.

If we were to choose a model to continue with, we would choose ridge regression because it performed better than the other two and because, in general, ridge regression analyzes multiple regression data that have multicollinearity. Because there are many independent variables in this dataset, there is a risk of there being multicollinearity, or correlations between variables. Further, this method is likely better for this problem because it is optimized for prediction, rather than reference.

## Conclusions

Initially, our goal was to determine if the frequency of Walmarts and community profile are good predictors of police killings in the United States. As we carried out the exploratory data analysis, namely merging the Walmart and county profile datasets, we discovered that we were working with a smaller dataset than anticipated and that this would certainly affect our results. As we proceeded with modeling, we recognized that ridge regression was performing best out of the linear, ridge, and lasso regression models so we decided to move forward with ridge and see how it performs on our test data. When performed on the test data, the ridge model resulted in an $R^2$ score of 0.14, indicating that the features we selected are not good predictors of police killing ratios per county. Of course, this low $R^2$ score could also be affected by various other factors, a main one being a lack of data points or a lack of features generally. As such, we can’t make any valid conclusions about the relationship between our variables of interest. Thus there’s no evidence that can lead us to conclude that the frequencies of walmarts and the demographics of our communities are good predictors of the frequencies of police killings.

### Potential Gaps:

As seen in our model performance, we were working with few data points, specifically 320 observations, which resulted in a highly inaccurate model. This was inevitable because there are only so many counties and Walmarts in the United States, and once the data was merged the amount of countries was not sufficient to create an accurate model. As such, the regression analysis would be significantly improved with a greater amount of data, though, this would be hard to find as we would have to have expanded our data internationally (to Canada) to access more counties, and even that may not be accurate because there may not be as great of a police violence issue in other countries. Thus, a lack of data concerning county-Walmarts was unavoidable.

There could also be a problem of endogeneity, wherein our predictor variables could be correlated with the error term in the model. This gap is a complex one to address within our project, but it’s important to recognize that there could potentially be another variable that is not included within our features that would affect both police killings and the number of Walmarts in counties. We sought to use race and poverty for our independent variables to focus on the number of Walmarts per person as a predictor of violence, but in doing so we could have excluded variables that may have led to endogeneity.

### Legal, Policy, and Ethical Implications:

Demonstrating a connection between community profiles and police violence highlights the potential issues with predictive policing. As shown in this project, there seems to be a direct correlation between police violence with race and socioeconomic status. If police are to use predictive policing, there will be a continuous positive feedback loop that continues to target these communities. It is essential that groups such as Mapping Police Violence continue to highlight the bias in police that leads to the deaths of innocent people so that we can make changes to our current system. Police must be held accountable for their actions and there must be a radical shift in how we handle crime in this country. 
