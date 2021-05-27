---
layout: page
title: Group 06
nav_order: 11
description:
---
# Building a Model to Predict One's Stance on Requiring Vaccinations in School Using Opinions on COVID-19 Policies

**By: Nikhil Patel, Ryan Little, Susan Pang**

For our project, we sought to construct a classifier that would determine one’s stance on requiring vaccinations in school based on their opinions concerning various topics related to the ongoing COVID-19 pandemic. Our underlying assumption was that there is an inherent relationship between one’s views on various COVID-19 policies and mandatory vaccinations. 

In order to build this classifier, we pulled data from the American National Election Studies 2020 time series. This extensive, academically-run national survey is conducted both before and after every presidential election. The data is publicly accessible for download. The study asks the same questions over time, but features specific questions related to the COVID-19 pandemic in 2020. There are questions related to respondents’ views on requiring vaccines in schools, the health benefits of vaccines, and the scientific evidence that vaccines cause autism. For 2020 specifically, there are questions related to the respondents’ views on the strictness of pandemic shutdowns, the speed of reopenings, the importance of science in driving policy decisions, and the efficacy of hydroxychloroquine in treating the virus. Based on responses to these questions and others, we aimed to build a classifier that would accurately predict whether a respondent is for or against requiring vaccinations in schools. We employed a suite of computational techniques to build this classifier, ranging from logistic regression to decision trees.

## Exploratory Data Analysis

**Data Source**

The ANES 2020 Time Series Study can be found at: [2020 Time Series Study](https://electionstudies.org/data-center/2020-time-series-study/)

**Ethical Concerns of Data Collection:**

As we are using data provided by the ANES without making any alterations besides renaming columns for clarity, there should be no real concern of violating ethical data collection principles, assuming the ANES hasn't somehow violated these principles when they were collecting it firsthand. However, this like isn't the case, as the data has been anonymized and we have followed the ANES rules on their website of not trying to discover the identities of any respondents, either intentionally or inadvertently.

**The Data**

![ANES Dataframe](ANES_df_pic.jpg)

**Description of data and variables chosen:**

We selected all variables related to vaccinations and COVID-19 and renamed the columns

![ANES Renamed Dataframe](ANES_vars.jpg)

* The first variable is a question asked to respondents on whether or not they think vaccines should be required in schools. The options are favor, oppose, or neither (denoted by different integers as responses), and there are a few other options that essentially correspond to "no answer" for various reasons. The next variable is a summary of the first, simply with more options of how strongly the respondent felt about their response, with different options such as strongly oppose, strongly in favor, etc. It is basically to gauge how strongly one feels about their answer to the previous question.

* The autism evidence feature is a question asked to respondents on whether or not they believe scientific evidence shows that vaccines are linked to autism. They could either answer yes or no, with other datapoints essentially corresponding to "no answer". 

* The vaccine in schools strength variable is measuring the magnitude of response to the original question of requiring vaccines in schools. It basically removes what they actually answered and simply gauges how strongly they felt about their answer in general, whether they answered yes or no. We use it because it measures how strongly a respondent feels about the topic.

* The household covid case variable asks if the respondent has had anyone in their household test positive for COVID-19. The options are yes/no or other possibilities corresponding to no answer. We hope to use this to see correlations between positive tests and other reponses.

* The vaccine health benefits feature asks if the respondent thinks the benefits of vaccines outweigh the risks, with the answer options being yes, no, or no difference. The vaccine health benefits summary is more in-depth, gauging how strongly they feel in whatever direction they answered, such as strongly agree, strongly oppose, etc.

* The putoff vaccines b/c of cost variable is a question that asks if the respondent knows anyone (including themself) that has put off a regular checkup or vaccine because of the cost. The options for answering are yes/no, with other entries in the data essentially corresponding to "no answer."

* The covid limits feature is a question that asks if the respondent thinks the limits their state placed on public activity because of COVID is too strict. The options range from far too strict to not nearly strict enough, with different intensities in between, including "about right".

* The reopening speed variable asks if the respondent thinks their state's reopening speed of relaxing stay-at-home orders is too fast, too slow, or about the right speed. 

* The science importance question asks the respondent how important they think science should be for making government decisions about COVID-19. The options range from not at all important to extremely important, with different checkpoints in between. 

* The COVID developed variable asks if the respondent thinks that COVID-19 was developed in a lab intentionally, with the answers being yes or no.

* The hydroxychloroquine effectiveness question asks if the respondent thinks that there is enough scientific evidence to show that hydroxychloroquine is an effective treatment for COVID-19, with the possible answers being yes or no.

**Visualizing Relationships**

Our model would seek to predict the 'vacc in schools' variable. In order to see if our model would be feasible, we made barplots visualizing the difference in opinion for pro-vaccination and anti-vaccination respondents for each feature. Clearly, features with larger differences in opinion between pro-vaccination and anti-vaccination respondents would be stronger predictor variables. Two of these visualizations are shown below.

![COVID Strictness Barplot](Covid_strictness.jpg)

These plots display that pro-vaccination respondents generally think the COVID limits were about right or not strict enough, while anti-vaccination respondents heavily lean toward saying they were too strict. The difference between the two groups is significant, so it will likely be a statistically significant predictor variable in our model.

![COVID Positive Barplot](Covid_positive.jpg)

The distributions for whether someone in their household tested positive for COVID-19 are essentially the same for pro-vaccination and anti-vaccination respondents. As a result, it will likely be a less significant predictor variable in our model.

**Data Limitations**

Considering the data-generating process, it is worth noting that there have been studies showing that people of certain parties or affiliations are hesitant to provide their true opinions as well as how strongly they feel about them, which is gauged in several of the features we use. This is something we need to keep in mind. This is always a risk with using surveys as we cannot ascertain the truthfulness of all the responses.

There is also possibility for non-response bias, as there are certain demographics and backgrounds that are more likely to be willing to participate in a survey of this length and depth, which could affect some of the distributions that we look into on public opinions - there needs to be caution taken in how we generalize the repsondents of this survey to the overall population.

## Modeling

**Cleaning the Data: One Hot Encoding**

The first major step before creating the model was to prepare the data by one-hot encoding the categorical variables. This is because even though the features are on a numerical scale of response, we don't necessarily want to treat them as linear magnitudes. For example, we wouldn't say a response of 2.0 is twice as much as 1.0 - it'd be better to simply treat them as their own categories.

![One Hot Encoded Data](onehotencode1.jpg)

**Class Imbalance**

In the dataset, there are significantly more people who favor vaccinations in schools than those who are opposed to it. This represents a significant class imbalance favoring pro-vaccination, which can be seen below. This may reflect current vaccination sentiments in the country, with most of the population in support, and a small, strong minority in opposition. This may also be a result of non-response bias in the data-generating process. It could be possible that people against vaccination are simply less likely to respond to the ANES survey. As collecting more data is not an option, we choose to focus on ROC curves, precision, and recall in our model selection in order to combat the potential issues arising from the class imbalance. Focusing solely on accuracy would be inadequate, as class imbalances often lead to models with uniformly high accuracy (this is because a dummy classifier that predicts that everyone will be pro-vaccine will be very accurate).

![Class Imbalance](classimbalance.jpg)

**Splitting the Data**

Next, the data was split into training, validation, and testing sets. We allocated 60% of the data to the training set, 20% to the validation set, and 20% to the testing set.

**Two Different Models**

In our efforts, we created two types of models - one only using COVID-related questions, such as reopening speed and importance of science in decision-making for COVID. We also created a model using the variables related to autism, health benefits, and health risks of vaccinations, as well as COVID-related question. We expected this model to perform well at classifying one's stance on vaccinations, but we hoped that the initial COVID-specific model would identify more nuanced and complex relationships between a respondent's stance on certain issues related to vaccines and COVID.

### Model 1: Using COVID-Related Features Only

**Different Classifiers Employed:**

For each model, we employed a suite of classifiers, including logistic regression, random forest classification, decision tree classification, vanilla bagging classification, and gradient boosting classification. The results for each are summarized below.

**Model 1: Logistic Regression**

![Logistic Results 1](log1summ.jpg)

![Logistic ROC 1](logistic_roc1.jpg)

The logistic regression model predicts that there is an overwhelming majority of people who favor vaccinations at school (3673 people), as opposed to a small minority who do not support it (74).

As seen from the accuracy score, the logistic regression model is fairly accurate. Therefore, we can assume the predictions of the model to be relatively reliable.

Based on the confusion matrix, there are a large number of false postives (391), meaning the model frequently predicts people support vaccines when they actually oppose them. There are not many false negatives (28), but there are also very few people who oppose vaccines in the dataset.

The AUC for this model is 0.548, which is only slightly better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). The closer the ROC curve comes to the 45 degree diagonal, indicating no predictive power, the worse its performance.

Furthermore, the model has high precision and recall for the pro-vaccination class, which is to be expected due to the class imbalance. However, it has very low recall for the anti-vaccination class (0.11). This means that it predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data. 

**Model 1: Random Forest Classifier**

![Random Forest Results 1](rf1summ.jpg)

![Random Forest ROC 1](rf_roc1.jpg)

Similar to the logistic regresssion model, the RFC model also predicts that there will be a majority of people who favor vaccinations at school (3637 people), as opposed to a small minority who do not support it (110).

As seen from the accuracy score, the RFC model appears to be fairly accurate, even more so than the logistic regression model. Therefore, we can assume the predictions of the model to be reliable, as it also has similar predictions to the logistic regression model.

Based on the confusion matrix, there are a large number of false postives (347), however there are less than in the logistic model. There are very few false negatives (20).

The AUC for this model is 0.600, which is still relatively low, but the best of all the models.

Furthermore, the model has high precision and recall for the pro-vaccination class, which is to be expected due to the class imbalance. However, it has relatively low recall for the anti-vaccination class (0.21). As stated above, this means that it predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data. Overall, it has the highest precision and recall numbers out of the models.

**Model 1: Decision Tree Classifier**

![Decision Tree Results 1](dt1summ.jpg)

![Decision Tree ROC 1](dt_roc1.jpg)

Unlike the previous two models, the DTC model only predicts there to be people who will support vaccinations in school. This seems unlikely, considering that it is not very realistic for everyone to have the same stance on vaccinations since it is such a controversial topic. Therefore, despite the model's high accuracy score, we cannot take the model's predictions to be reliable.

Based on the confusion matrix, there are a very large number of false postives (437), meaning the model frequently predicts people support vaccines when they actually oppose them. There are 0 true negatives, which indicates the model performs poorly at predicting people who oppose vaccines in schools.

The AUC for this model is 0.500, which is the same as the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). This is very bad and indicates the model has no real predictive power.

Furthermore, the model has high precision and recall for the pro-vaccination class. However, it has a precision and recall of 0 for the anti-vaccination class, due to predicting 0 instances of anti-vaccination. 

**Model 1: Vanilla Bagging**

![Vanilla Bagging Results 1](vb1summ.jpg)

![Vanilla Bagging ROC 1](vb_roc1.jpg)

Similar to the logistic regresssion and RFC model, the vanilla bagging model also predicts that there will be a majority of people who favor vaccinations at school (3687 people), as opposed to a small minority who do not support it (60).

As seen from the accuracy score, the vanilla bagging model appears to be fairly accurate. Therefore, we can assume the predictions of the model to be reliable, as it also has similar predictions to the logistic regression and RFC models.

Based on the confusion matrix, there are a large number of false postives (394), meaning the model frequently predicts people support vaccines when they actually oppose them. There are very few false negatives (17), but there are also very few people who oppose vaccines in the dataset.

The AUC for this model is 0.547, which is only slightly better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5).

Furthermore, the model has high precision and recall for the pro-vaccination class. However, it has very low recall for the anti-vaccination class (0.10). This means that it predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

**Model 1: Gradient Boosting Classifier**

![Gradient Boosting Results 1](gbc1summ.jpg)

![Gradient Boosting ROC 1](gbc_roc1.jpg)

Similar to the logistic regresssion and RFC model, the boosting model also predicts that there will be a majority of people who favor vaccinations at school (3737 people). However, only 10 people are predicted to be in opposition to vaccines, which is much less than was predicted for previous models. Despite the model's relatively high accuracy score, we cannot take the model's results to be reliable due to the outlier results.

Based on the confusion matrix, there are a very large number of false postives (430), meaning the model frequently predicts people support vaccines when they actually oppose them. There are extremely few false negatives (3), but also very little true negatives, as noted above.

The AUC for this model is 0.508, which is barely better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5)

The model has high precision and recall for the pro-vaccination class. However, it has extremely low recall for the anti-vaccination class (0.02). It predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

**Model 1: Validation**

In summary, the logistic classifier performed the best on the validation set. It had the highest accuracy and maintained a high precision and recall for the pro-vaccination class. While the precision for the anti-vaccination class was relatively high, the recall remained low. Like above, this means that it predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data. This is a result of the large class imbalance in the data. All other models had lower validation accuracy, with similar numbers for precision and recall, indicating that they would not generalize well to unseen data. The results are shown below:

![Logistic Validation Results 1](logval1summ.jpg)

**Out of all the above choices for Model 1, the Random Forest Classifier performed the best on the training data. It had the highest accuracy, area under the ROC curve, and precision and recall, indicating that it balanced true positives and false positives well and had strong predictive power. However, it did not perform well on the validation set, indicating that it likely would not generalize well to the test set due to overfitting. As a result, we moved forward using the Logistic Regression model on the test data. It performed the next best on the training data and had the best validation performance, indicating that it would likely generalize better to the test data.**

**Examining Feature Importance**

Random forest models allow easy access to the predictive importance of each of the features. With these predictive importances, we can see which features contribute the most to one's stance on requiring vaccinations. The feature importances are visualized below:

![Feature Importance 1](featureimportance1.jpg)

The top 5 most important features appear to be:
* science importance_5.0: Opinion that science should be extremely important for decisions about COVID-19
* covid limits_1.0: Opinion that limits place on public activity due to COVID-19 are far too strict
* science importance_1.0: Opinion that science should be not at all important for decisions about COVID-19
* covid developed_2.0: Opinion that COVID-19 was not developed intentionally in a lab
* reopening speed_2.0: Opinion that COVID-19 reopenings are happening too slowly

It appears that features relating to disapproval or mistrust concerning COVID-19 are relatively more important in this model.

### Model 2: Using Both Vaccination and COVID-Related Features

**Different Classifiers Employed:**

For each model, we employed a suite of classifiers, including logistic regression, random forest classification, decision tree classification, vanilla bagging classification, and gradient boosting classification. The results for each are summarized below.

**Model 2: Logistic Regression**

![Logistic Results 2](log2summ.jpg)

![Logistic ROC 2](log_roc2.jpg)

The second logistic regression model predicts that there will be a majority of people who favor vaccinations at school (3481 people), as opposed to a minority who do not support it (266). As opposed to our previous models where we only took into account COVID-related opinions, now we are also considering general opinions on vaccinations too. As a result, the number of people predicted to support vaccinations at school went down, while the number predicted to not support vaccinations at school went up.

As seen from the accuracy score, this second logistic regression model is even more accurate than when we modelled based on COVID-related opinions only. Therefore, we can assume the predictions of the model to be reliable.

Based on the confusion matrix, there are a large number of false postives (232), meaning the model still frequently predicts people support vaccines when they actually oppose them. This makes sense considering the small number of vaccine opposers in the dataset. There are not many false negatives (61).

The AUC for this model is 0.725, which is much better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). The closer the ROC curve comes to the 45 degree diagonal, indicating no predictive power, the worse its performance.

The model has high precision and recall for the pro-vaccination class. It maintains relatively high precision for the anti-vaccination class, but suffers from low recall. This indicates that it predicts relatively few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

**Model 2: Random Forest Classifier**

![Random Forest Results 2](rfsumm2.jpg)

![Random Forest ROC 2](rf_roc2.jpg)

The RFC model predicts that there will be a majority of people who favor vaccinations at school (3401 people), as opposed to a minority who do not support it (346). As opposed to our previous models where we only took into account COVID-related opinions, now we are also considering general opinions on vaccinations too. As a result, the number of people who support vaccinations at school went down, while the number who do not support vaccinations at school went up.

As seen from the accuracy score, this RFC model has the highest accuracy of all models.

Looking at the confusion matrix, there are a relatively small number of false postives (119) compared to all other models.

The AUC for this model is 0.860, which is much better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). This is the highest AUC of all models, indicating it has strong predictive power.

The model has relatively high precision and recall for both the pro-vaccination and anti-vaccination classes. 

**Model 2: Decision Tree Classifier**

![Decision Tree Results 2](dtcsumm2.jpg)

![Decision Tree ROC 2](dtc_roc2.jpg)

Unlike the second logistic regression or RFC models, the second DTC model only predicts there to be people who will support vaccinations in school (3747 people). This seems unlikely, considering that it is not very realistic for everyone to have the same stance on vaccinations since it is such a controversial topic. Despite the model's somewhat high accuracy score, we cannot take the model's predictions to be reliable.

Based on the confusion matrix, there are a very large number of false postives (437), meaning the model frequently predicts people support vaccines when they actually oppose them. There are 0 true negatives, which indicates the model performs poorly at predicting people who oppose vaccines in schools.

The AUC for this model is 0.500, which is the same as the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). This is very bad and indicates the model has no predictive power.

Furthermore, the model has high precision and recall for the pro-vaccination class. However, it has a precision and recall of 0 for the anti-vaccination class, due to predicting 0 instances of anti-vaccination.

**Model 2: Vanilla Bagging**

![Vanilla Bagging Results 2](vbsumm2.jpg)

![Vanilla Bagging ROC 2](vb_roc2.jpg)

Similar to the logistic regresssion and RFC model, the second vanilla bagging model also predicts that there will be a majority of people who favor vaccinations at school (3463 people), as opposed to a minority who do not support it (284).

As seen from the accuracy score, this second vanilla bagging model is even more accurate than when we modelled based on COVID-related opinions only. It has relatively high accuracy (0.932), but it does not perform as well as the Random Forest Classifier.

Looking at the confusion matrix, there are a relatively large number of false postives (203), meaning the model frequently predicts people support vaccines when they actually oppose them. This makes sense considering the small number of vaccine opposers in the dataset. There are not many false negatives (50).

The AUC for this model is 0.760, which is much better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). This indicates that the model has strong predictive power.

The model has high precision and recall for the pro-vaccination class. It maintains relatively high precision for the anti-vaccination class, but suffers from low recall. This indicates that it predicts relatively few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

**Model 2: Gradient Boosting Classifier**

![Gradient Boosting Results 2](gbcsumm2.jpg)

![Gradient Boosting ROC 2](gbc_roc2.jpg)

Similar to the logistic regresssion, RFC and vanilla bagging models, the boosting model also predicts that there will be a majority of people who favor vaccinations at school (3551 people), as opposed to a minority who do not support it (196). This is different from the first boosting model done above, which had an outlier result for the people who did not support vaccinations at school. The results for this second boosting model are much more similar to the second logistic regression and vanilla bagging model. It has a high accuracy score (0.916), so we can assume the result to be reliable.

Looking at the confusion matrix, there are a relatively large number of false postives (278), meaning the model frequently predicts people support vaccines when they actually oppose them. Again, this makes sense considering the small number of vaccine opposers in the dataset. There are not many false negatives (37).

The AUC for this model is 0.676, which is somewhat better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). This indicates that the model has relatively strong predictive power, but it does not perform that well compared to other models.

Additionally, this model has high precision and recall for the pro-vaccination class. It has relatively high precision for the anti-vaccination class, but suffers from low recall. As above, this indicates that it predicts relatively few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

**Model 2: Validation**

In summary, the logistic classifier performed the best on the validation set. It had the highest accuracy and maintained a high precision and recall for the pro-vaccination class. While the precision for the anti-vaccination class was relatively high, the recall remained low. Like above, this means that it predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data. This is a result of the large class imbalance in the data. All other models had lower validation accuracy, with similar numbers for precision and recall, indicating that they would not generalize well to unseen data. Its results are shown below:

![Logistic Validation Results 2](logval2summ.jpg)

**Once again, out of all the above choices for Model 2, the Random Forest Classifier performed the best on the training data by far. It had the highest accuracy and area under the ROC curve, indicating the it balances true positives and false positives well and has strong predictive power. However, it did not perform well on the validation set, indicating that it likely would not generalize well to the test set due to overfitting. As a result, we moved forward using the Logistic Regression model on the test data. It performed the next best on the training data and had the highest validation accuracy, indicating that it would likely generalize better to the test data.**

**Examining Feature Importance**

Random forest models allow easy access to the predictive importance of each of the features. With these predictive importances, we can see which features contribute the most to one's stance on requiring vaccinations. The feature importances are visualized below:

![Feature Importance 2](featureimportance2.jpg)

The top 5 most important features appear to be:
* vaccine health benefits_1.0: Opinion that health benefits of vaccines outweight risks
* autism evidence_1.0: Opinion that most scientific evidence shows childhood vaccines cause autism
* vaccine health benefits_2.0: Opinion that health risks of vaccines outweight benefits
* austism evidence_2.0: Opinion that most scientific evidence shows childhood vaccines do not cause autism
* science importance_5.0: Opinion that science should be extremely important for decisions about COVID-19

It appears that vaccine-specific features are more important in this model than COVID-specific features.

## Testing

**As determined above, logistic regression was found to perform the best overall considering both the training and validation sets for both model types. As a result, it will be used in testing.**

### Model 1: Using COVID-Related Features Only

![Model 1 Test Results](model1testsumm.jpg)

![Model 1 ROC](model1testroc.jpg)

**Model 1: Results**

The logistic regression model performed well on the test data. As seen from the accuracy score of 0.879, the model had relatively strong predictive power on the test set.

Based on the confusion matrix, there are a relatively large number of false postives (146). The model frequently predicts that people support vaccines when they actually oppose them. This makes sense considering the small number of vaccine opposers in the dataset as a whole. Furthermore, there are extremely few false negatives (5).

The AUC for this model is 0.541, which is not much better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). As explained, the closer the ROC curve comes to the 45 degree diagonal, indicating no predictive power, the worse its performance. As the ROC curve lies close the this 45 degree line, we can conclude that its predictive power is not much better than randomly guessing.

Additionally, the model has high precision and recall for the pro-vaccination class, which is to be expected due to the class imbalance. It has relatively high precision for the anti-vaccination class, but suffers from very low recall (0.09). This means that it predicts very few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

While we were far from perfection, this model shows that you can predict one's stance on requiring vaccinations in schools somewhat well based solely on their views regarding COVID-19 policies and topics. However, the model clearly struggles with predicting anti-vaccination opinions, largely due to the class imbalance presence in the ANES data.

### Model 2: Using Both Vaccination and COVID-Related Features

![Model 2 Test Results](model2testsumm.jpg)

![Model 2 ROC](model2testroc.jpg)

**Model 2: Results**

Ultimately, the logistic regression model performed well on the test data. As seen from the accuracy score of 0.917, the model had strong predictive power on the test set. As expected, the second logistic regression model is even more accurate than when we modeled based on COVID-related opinions only. The addition of general vaccination opinions to the COVID model resulted in even stronger performance.

Based on the confusion matrix, there are a relative few false postives (84). While not many, the model still frequently predicts people support vaccines when they actually oppose them. This makes sense considering the small number of vaccine opposers in the dataset as a whole. Furthermore, there are very few false negatives (20).

The AUC for this model is 0.728, which is much better than the AUC for guessing 1 (supports vaccines) every time (AUC would equal 0.5). As explained before, the closer the ROC curve comes to the 45 degree diagonal, indicating no predictive power, the worse its performance.

Furthermore, the model has high precision and recall for the pro-vaccination class, which is to be expected due to the class imbalance. It has relatively high precision for the anti-vaccination class, but suffers from mediocre recall (0.47). It predicts relatively few instances of anti-vaccination, but most of its predictions are correct when compared to the training data.

While not perfect, our modeling has shown that you can effectively predict one's stance on requiring vaccinations in schools based on their views regarding COVID-19 policies and other vaccine-related topics. However, due to class imbalances in the data, the model struggles with predicting anti-vaccination stances.

## Conclusions

The aim of our project was to determine if our classifiers could predict whether an individual supports mandatory vaccinations in schools based on their opinions about COVID-19 and vaccinations in general. The question posed by this data analysis project is important because it relates to an ongoing political debate in the United States and throughout the world. Claims based on bad science have resulted in a strong anti-vaccination movement throughout the country, which may be bolstered by the COVID-19 pandemic.

In terms of modeling results, we can see that despite the baseline model of always predicting someone to be pro-vaccination having very high accuracy (due to the large proportion of pro-vaccination respondents in the data), our models were able to improve upon this metric as well as others, including precision and recall. Overall, our second model, which included both vaccination and COVID-related features, performed better than the first. This was to be expected. It performed significantly better on the test set, implying that adding these features did not simply lead to overfitting. Rather, it improved the quality of the model on unseen data. Furthermore, the AUC of the second model was well above that of the first model and the baseline model, implying that there is a decent amount of predictive power in our model. While far from perfect, our modeling has shown that you can effectively predict one's stance on requiring vaccinations in schools based on their views regarding COVID-19 policies and other vaccine-related topics.  

Class imbalance was a serious issue with this dataset. A significant amount of the ANES respondents were pro-vaccination, with only a small portion being strictly anti-vaccination. This may reflect current vaccination sentiments in the country, with most of the population in support, and a small, strong minority in opposition. This may also be a result of non-response bias in the data-generating process. It could be possible that people against vaccination are simply less likely to respond to the ANES survey. As collecting more data is not an option, we chose to focus on ROC curves, precision, and recall in our model selection in order to combat the potential issues arising from the class imbalance. Focusing solely on accuracy would be inadequate, as class imbalances often lead to models with uniformly high accuracy (this is because a dummy classifier that predicts that everyone will be pro-vaccine will be very accurate). However, even after using precision, recall, and ROC curve metrics, our models still largely struggled with predicting anti-vaccination stances.

Our research has multiple implications. One potential implication is that the COVID-19 pandemic has exacerbated anti-vaccination sentiments in the United States. As shown by the models, COVID-related opinions are correlated with stances on vaccinations. It is possible that controversy surrounding COVID-19 vaccinations will spread to other vaccines. As a result, we may see tensions rise as policies are introduced to require vaccinations in schools in the future. On the other hand, a successful COVID vaccination rollout could potentially allay a large amount of anti-vaccination sentiment in the United States and lead to more accepted standards for vaccination.

Analyzing the feature importance for the second model showed that vaccine-specific features were generally more important than COVID-specific features. The first model showed that some predictive power was possible using COVID-related features only, but a large amount of that success was due to the class imbalance in the data. Moving forward, the important features show that it is especially salient to tackle public opinions concerning the health risks of vaccines and their correlation with autism in order to reduce anti-vaccination sentiment. This could help guide education campaigns and lobbyists working on mandatory vaccination policies.

**Future Steps**

There is a clear majority of pro-vaccination respondents in the ANES data. A future step in improving the model could be the use of SMOTE - Synthetic Minority Oversampling Technique. This technique generates synthetic data to help train classification models on imbalanced data. More simply, random undersampling of the positive class (pro-vaccination) could help to solve the issue of the data imbalance. In the future, more data could also be collected to solve the class imbalance present in the data.
