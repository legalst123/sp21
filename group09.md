---
layout: page
title: Group 09
nav_order: 14
description:
---
## Do the demographics of drug-related mortalities in the U.S. affect legalization-related sentiments expressed in press releases and news articles?

### Shalini Haupt, Zaina Siyed, Winston Tran, Nicholas Ha

## 1) RESEARCH QUESTION AND MOTIVATION

Our research question seeks to understand the nexus between the drug epidemic in the U.S. and the contextual tone set around it in press releases and news articles. In the last 30 years, the overall mortality rate for unintentional drug poisonings has grown exponentially, and diffferent drugs have contributed to this exponential growth at different times. From 2005-2006, cocaine was the leading cause, followed by prescription opioids, then heroin, then synthetic opioids such as fentanyl. Moreover, the demographics of individuals afflicted by drug overdoses has also shown substantial variability. Until 2010, most drug overdoses were in 40- to 50-year-old individuals from cocaine and perscription drugs. Subsequently, deaths from heroin and fentanyl began to affect younger individuals in the 20- to 40-year-old age group. In terms of race, death rates for White individuals have generally exceeded those for Black individuals for all opioids, while rates have been much greater among Black individuals for cocaine ([Source 1](http://www2.erie.gov/health/sites/www2.erie.gov.health/files/uploads/pdfs/articles/Changing%20dynamics%20of%20the%20drug%20overdose%20epidemic%20in%20the%20United%20States%20from%201979%20through%202016.pdf)). While there is an abundance of research on the changing nature of the drug epidemic in the U.S., our research seeks to connect these tangible, real-world demographics of drug-related mortalities with messaging surrounding the legalization of drugs...

In 1971, when drugs were perceived as symbols of youthful rebellion, social upheaval, and political dissent, President Richard Nixon declared a war on drugs. As a result of Nixon's declaration, the U.S. assumed a harsh stance on drugs—from increasing the size and presence of federal drug control agencies to pushing through measures such as mandatory sentencing and no-knock warrants. In 1980, for example, 580,900 individuals were arrested on drug-related charges in the U.S. By 2014, that number had increased to 1,561,231 ([Source 2](https://www.cato.org/policy-analysis/four-decades-counting-continued-failure-war-drugs)). Today, however, we are starting to see a shift toward more treatment-based rather than punishment-based drug policy, and discussions surrounding the legalization and decriminilazation of drug use are starting to predominate. For example, legalization of marijuana has become a highly debated topic in nearly every U.S. state. In 2012, Colorado and Washington became the first two states to legalize the recreational use of cannabis following the passage of Amendment 64 and Initiative 502, respectively. Since then, national support for marijuana legalization went from 43% in 2012 to 55% in 2014 ([Source 3](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5509359/)). In recent years, there has also been a push from punishment-based to treatment-based drug policy for more serious drugs such as opioids, cocaine, and heroin. In response to the opioid epidemic, for example, policymakers implemented safe consumption spaces designed to allow drugs to be consumed under supervision to improve medical outcomes for high-risk patients. These types of treatment-based iniatives represent a shift away from efforts to combat drug use through mass incarceration.

Taking all this historical context together, there are effectively two ways to analyze the drug epidemic in U.S. states—through a mortality lens and though a policy lens. Our research aims to understand the relationship between the two. By analyzing how the demographics of drug-related mortalities in the U.S. impact respective messaging surrounding the legalization of drugs, we hope to understand whether the race, age, gender, etc. of individuals afflicted by drug overdoses has an effect on how the legalization of drugs is portrayed by lawmakers and media outlets.

## 2) DATA ACQUISITION

We will use both quantitative mortality data and qualitative text data (spanning from 1999-2019) to explore our research question. Here are the three datasets we will be working with:

_I. [QUANTITATIVE MORTALITY DATA SOURCE](https://wonder.cdc.gov/wonder/help/mcd.html)_<br>
CDC Wonder, or Wide-Ranging Online Data for Epidemiological Research, is an integrated information system designed to provide the general public with access to specific and detailed information from the CDC. For our research, we will use the CDC's Multiple Cause of Death, 1999-2019 dataset. We will group by year and state, such that each observation represents, for example, the number of drug-related deaths in CA in 1999. Within each state-year combination, we will also include features related to the demographics of drug-related mortalities. Our 7 demographics of interest are as follows:
 - __Age group__: 15-24 yrs, 25-34 yrs, 35-44 yrs, 45-54 yrs, 55-64 yrs, 65-74 yrs, 75-84 yrs, 85+ yrs
 - __Race__: White, Black or African American, American Indian or Alaska Native, Asian or Pacific Islander
 - __Cause of death__: Other opioids, Methadone, Cocaine, Heroin, Cannabis (derivatives)
 - __Gender__: Male, Female
 - __Place of death__: Decedent's home, Medical facility - Outpatient or ER, Medical facility - Inpatient, Medical facility - Dead on arrival, Medical facility - Status unknown, Hospice facility, Place of death unknown, Other
 - __Injury intent__: Unintentional, Suicide, Non-injury, no intent classified, Undetermined
 - __Weekday__: Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday

_II. [QUALITATIVE TEXT DATA SOURCE #1](https://drugpolicy.org/press-release)_<br>
Press releases from state governments compiled by The Drug Policy Alliance will serve as our first text data source. These publications express the viewpoints of state policymakers surrounding the legalization and decriminalization of various drugs.

_III. [QUALTITATIVE TEXT DATA SOURCE #2](https://developer.nytimes.com/docs/articlesearch-product/1/overview)_<br>
Articles published by The New York Times on topics of drug legalization will serve as our second text data source. Since we are interested in understanding the contextual tone set around the drug epidemic by both lawmakers and media outlets, these articles will help us analyze the latter—how stances on drugs expressed by media outlets have changed over time.

To acquire our text data, we used three different data acquisition methodologies:
 - CDC data: online query
 - Drug Policy Alliance data: custom-built web scrape (9,920 texts pulled)
 - New York Times data: API pull (43,785 texts pulled)

## 3) DATA CLEANING

Our data cleaning process involved recoding the mortality data and preprocessing the text data...

### -- RECODING MORTALITY DATA --

The raw output of the CDC query gives a column for every category of interest, a separate column for the number of deaths, and a row for each unique combination of demographic categories. For example, the number of white individuals who died from opioids in Alabama in 2000 in the 35-44 age group. As such, the granularity of the mortality dataset was extremely fine. Here, is what the mortality data looked like before recoding it:

| State | Year | Age group | Race | ... | Cause of death | Deaths |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| alabama | 2000 | 35-44 years | White | ... | Other opioids | 15 |
| alabama | 2001 | 35-44 years | White | ... | Other opioids | 15 |
| alabama | 2002 | 35-44 years | White | ... | Other opioids | 15 |
| alabama | 2002 | 45-54 years | White | ... | Methadone | 15 |
| alabama | 2004 | 15-24 years | White | ... | Methadone | 15 |
| ... | ... | ... | ... | ... | ... | ... | 
| wisconsin | 2019 | 55-64 years | White | ... | Methadone | 15 |
| wisconsin | 2019 | 55-64 years | White | ... | Cocaine | 15 |
| wisconsin | 2019 | 65-74 years | White | ... | Other opioids | 15 |
| wyoming | 2010 | 45-54 years | White | ... | Other opioids | 15 |
| wyoming | 2017 | 45-54 years | White | ... | Other opioids | 15 |

In order to create the demographic features for our model, we had to bring the granularity back up to the level of simple state-year combinations. We wrote a custom "expand" function to achieve this by:
1. Expanding each unique value in each category into separate columns
2. Taking the number of deaths in that unique value
3. Grouping by year and state (aggregating by sum of deaths)

So, for example, instead of a "Race" column with categorical entries, we now have four separate columns for each race category, populated by the number of deaths in each race in each state-year combination. Here is what the mortality data looked like after recoding it:

| State | Year | 35-44 years | 45-54 years | ... | White | Black or African American |
| ----------- | ----------- | ----------- | ----------- | ----------- | ----------- | ----------- |
| alabama | 2000 | 15 | 0 | ... | 15 | 0 |
| alabama | 2001 | 10 | 0 | ... | 10 | 0 |
| alabama | 2002 | 11 | 13 | ... | 24 | 0 |
| alabama | 2004 | 31 | 24 | ... | 62 | 11 |
| alabama | 2005 | 21 | 17 | ... | 59 | 0 |
| ... | ... | ... | ... | ... | ... | ... | 
| wisconsin | 2017 | 268 | 207 | ... | 868 | 117 |
| wisconsin | 2018 | 201 | 188 | ... | 738 | 92 |
| wisconsin | 2019 | 218 | 189 | ... | 690 | 102 |
| wyoming | 2010 | 0 | 13 | ... | 13 | 0 |
| wyoming | 2017 | 0 | 12 | ... | 12 | 0 |

### -- PREPROCESSING TEXT DATA --

After combining the DPA press releases and NYT articles, we wrote a custom "clean_text function to perform several transformations on the text data:
 - lower the text
 - tokenize the text (split the text into words) and remove the punctuation
 - remove useless words that contain numbers
 - remove useless stop words like 'the', 'a' ,'this' etc.
 - Part-Of-Speech (POS) tagging: assign a tag to every word to define if it corresponds to a noun, a verb etc. using the WordNet lexical database
 - lemmatize the text: transform every word into their root form (e.g. rooms -> room, slept -> sleep)
 - remove texts with 3 or fewer words

## 3) EXPLORATORY DATA ANALYSIS

Before we explain our models, let's understand our mortality and text data a bit better...

### -- VISUALIZATIONS FOR QUANTITATIVE MORTALITY DATA --

![alt text](Images/deaths_by_drug.png)

In 1999, deaths due to cannabis, heroin, and methadone were in the 0-2,000 range, while deaths due to opioids and cocaine were in the 2,000-4,000 range. From 1999 to 2019, deaths due to marijuana stayed relatively consistent, with a slight increase from 2016 to 2019 (when marijuana was legalized in many states). Heroin- and opioid-related deaths, on the other hand, skyrocketed from 2010 to 2016. Cocaine-related deaths show a more variabe trajectory—increasing from 1999 to 2006, decreasing from 2006 to 2010, then increasing from 2010 to 2019. Finally, deaths due to methadone (a prescription medication used as a substitute drug in the treatment of morphine and heroin addiction) increased from ~300 in 1999 to ~5,000 in 2007, then decreased until 2019.

![alt text](Images/deaths_by_race.png)

While drug-related deaths of American Indian and Asian individuals were extremely low from 1999 to 2019, deaths of White individuals were extremely high (increasing consistently from ~6,400 in 1999 to ~38,500 in 2017). Deaths of Black individuals hovered in the 1,000-2,000 range from 1999 to 2013, then increased to ~7,400 in 2019. With our model, we hope to understand how increases and decreases in deaths in a specific race differentially affect legalization sentiments expressed in press releases and news articles.

![alt text](Images/prop_deaths_by_age.png)

Finally, looking at the proportions of deaths within each cause of death segmented by age group, we see some interesting patterns. Under cocaine, the highest represented age groups are 35-44 years and 45-54 years. Under heroin, the highest represented age group is 25-34 years. And under opioids, the highest represented age group is 45-54 years. This means that individuals affected by heroin overdoses are, on average, younger than those affected by cocaine or opioid overdoses. In terms of how these patterns relate to our research, we hypothesize that when mortalities affect younger individuals, we will see a greater impact on sentiments expressed in news articles and press releases, due to the fact that lawmakers and media outlets often take a stronger stance when the problem at hand affects younger generations.

### -- VISUALIZATIONS FOR QUALITATIVE TEXT DATA --

Below, we show word clouds for cocaine-related texts and opioid-related texts...

__<div align="center">Word cloud for cocaine-related texts__

![alt text](Images/cocaine_word_cloud.jpg)

__<div align="center">Word cloud for opioid-related texts__

![alt text](Images/opioids_word_cloud.jpg)

Comparing the word clouds for texts related to cocaine vs. opioids, we see some patterns in terms of legalization sentiments. The word cloud for texts related to cocaine features words such as "mandatory minimum," "DEA," "criminal," "law enforcement," "crack," and "African American." These words suggest that the stance on cocaine use expressed in news articles and press releases is generally more harsh—discussions involving mandatory minimum sentencing, the Drug Enforcement Agency, etc. point to a more anti-legalization/pro-criminalization sentiment surrounding cocaine use. The word cloud for texts related to opioid use tells a different story. Words such as "public health," "expand access," "consumption space," "harm reduction", "save life", and "good samaritan" suggest that the stance on opioid use expressed in news articles and press releases is generally less harsh. Discussions surrounding safe consumption spaces and the Good Samaritan law (which provides limited protection from arrest, charge, and prosecution for people who seek emergency medical assistance at the scene of a suspected drug overdose) point to a more pro-legalization/anti-criminalization sentiment surrounding opioid use. Later, you will see how we quantified these differences in sentiments with our text classification model.

## 4) TEXT FEATURE ENGINEERING

Before running our final model predicting text legalization sentiments based on demographics of mortalities, we had to classify each text as pro-legalization vs. anti-legalization:
 - 1: pro-legalization (or anti-criminalization)
 - 0: anti-legalization (or pro-criminalization)
 
In order to classify texts as 1 or 0, we needed to engineer numerical text features. To do so, we added the following features using a variety of text-analysis packages:
 - Vader sentiment values
     - neutrality score
     - positivity score
     - negativity score
     - overall score
 - Doc2Vec representation vectors
     - creates a numerical vector representation of the corpus
 - TF-IDF values
     - computes the number of times each word appears in the corpus
     - computes the relative importance of each word depending on its frequency
 - Simple metrics
     - number of characters in the text
     - number of words in the text

Let's take a look at some visualizations of these text features...

![alt text](Images/sentiment_distribution.png)

Looking at the difference in compound sentiment scores between NYT texts and DPA texts, we see that NYT texts generally display more neutral sentiments (unimodal at compound=0) while DPA texts generally display more extreme sentiments (bimodal at compound=-1 and compound=1). This could be the case because drug-related NYT publications generally report incidents and policy updates as a neutral third-party source, while the collection of press releases aggregated by the DPA reflect more extreme views held by policymakers.

![alt text](Images/word_count_by_drug.png)

The above box plot summarizes the word counts of texts pulled for each drug. This box plot reveals the discrepancies in the amount of text data available on each drug. For example, not only were we able to pull the highest number of articles and press releases for marijuana, the word count for marijuana-related texts is also generally higher than it is for other drugs. For example, the upper quartile of the marijuana word count boxplot extends almost 900 words above the other boxplots. This data discrepancy, while unfortunate, was unavoidable considering marijuana legalization and decriminalization is discussed much more frequently in press releases and news articles.

## 5) CLASSIFYING LEGALIZATION SENTIMENT USING A RANDOM FOREST CLASSIFIER

In order to classify texts according to their stance on drugs (based on the text features we engineered above), we needed to have some training data to train the model on. So, we hand-tagged a random sample of 200 texts. Afterward, we trained a Random Forest classifier on our hand-tagged texts, then predicted the legalization sentiments of the un-tagged texts. We chose to use a Random Forest classifier because it is 1) fairly robust on smaller datasets and 2) easy to interpret by looking at the relative importance of features based on their location depthwise in the tree. Here are the top 20 features of our Random Forest classifier, ordered by feature importance in terms of predicting legalization sentiment:

| Text feature | Importance |
| ----------- | ----------- |
| word_week | 0.010400 |
| word_local | 0.009406 |
| word_massachusetts | 0.008819 |
| neg | 0.008667 |
| word_president | 0.007288 |
| word_amendment | 0.006608 |
| word_veto | 0.006592 |
| word_time | 0.006559 |
| word_comment | 0.006530 |
| word_term | 0.006472 |
| word_washington | 0.006306 |
| word_white | 0.006155 |
| word_anti | 0.006007 |
| word_back | 0.005989 |
| word_colorado | 0.005744 |
| word_arizona | 0.005505 |
| word_disparity | 0.005330 |
| word_talk | 0.005323 |
| word_live | 0.005222 |
| word_mexico | 0.005112 |

We see that the the text features with the highest predictive power in terms of legalization sentiment are a combination of words and sentiment scores. According to our model, the words which have the highest predictive power are: week, local, massachusetts, president, amendment, veto, time, comment, term, washington, white, anti, back, colorado, arizona, disparity, talk, live, and mexico. Words like "veto" and "anti" make sense because theoretically these words would appear more often in texts that represent an anti-legalization/pro-criminalization sentiment. The word "white" likely suggests that their is an association between race (specifically white individuals) and legalization sentiment. Although it's not clear in which direction, we would guess that texts in which white individuals are discussed are more often pro-legalization/anti-criminalization. Additionally, it's interesting to see that the mention of specific states—massachussets, washington, colorado, and arizona—is also a predictive text feature. In addition to words, we see that the negative sentiment score ("neg") is an important predictor of drug-related legalization sentiments. Note that some of the text features we engineered, such as the neutral, positive, and compound sentiment scores as well as the number of words, number of characters, and Doc2Vec vectors, don't appear in our top 20 most predictive features.

Let's zoom in a bit and see how our model actually tagged specific texts...

__<div align="center">Text classified as having the highest legalization sentiment__

> congress and numerous states are moving to legalize marijuana this year, building on positive outcomes in alaska, california, colorado, maine, massachusetts, nevada, oregon, washington state, and washington, d.c. in vermont, governor phil scott is expected to sign the state’s marijuana legalization bill today, making it the 9th state to legalize marijuana – and the first to do so via state legislature – in a rebuke to attorney general jeff sessions, who rescinded obama-era guidance this month allowing states to implement their own marijuana laws with limited federal interference. on tuesday, a new report by the drug policy alliance, from prohibition to progress: a status report on marijuana legalization, will show how and why marijuana legalization is working so far. on tuesday, january 23 at 1pm (et) / 10am (pt), dpa will host a press teleconference to discuss the report’s findings with key policymakers and elected officials: roseanne scotti, new jersey state director, drug policy alliance (moderator) jolene forman, staff attorney, drug policy alliance (report author) reggie jones-sawyer, california state assembly member and author of the legal cannabis protection act colorado state representative jonathan singer shaleen title, commissioner, massachusetts cannabis control commission members of the press are invited to join tuesday’s teleconference. please contact tony newman for call-in info.: 646-335-5384 from prohibition to progress finds that states are saving money and protecting the public by comprehensively regulating marijuana for adult use. there have been dramatic decreases in marijuana arrests and convictions, saving states millions of dollars and preventing the criminalization of thousands of people. marijuana legalization is having a positive effect on public health and safety. youth marijuana use has remained stable in states that have legalized. access to legal marijuana is associated with reductions in some of the most troubling harms associated with opioid use, including opioid overdose deaths and untreated opioid use disorders. dui arrests for driving under the influence, of alcohol and other drugs, have declined in colorado and washington, the first two states to legalize marijuana.

__Explanation of model tagging (high legalization sentiment)__: The above text was tagged as having the highest (i.e. most positive) legalization sentiment by our Random Forest classifier, with a predicted legalization sentiment score of 0.99. If we look at the actual content of the text, it's very clear why. This piece of text discusses a movement in congress and states like Vermont to legalize marijuana. It discusses the positive outcomes in states that have already legalized marijuana, such as fewer marijuana-related arrests and convictions (with the prevalence of youth marijuana use staying consistent). Additionally, this text also discusses the relationship between marijuana legalization and positive outcomes related to the opioid epidemic. More specifically, that access to marijuana has reduced some of the more troubling aspects of the opioid epidemic, such as reducing opioid deaths and untreated opioid use disorder. This text very clearly expresses pro-legalization/anti-criminalization sentiments regarding drug use and treatment, and it serves as a good sanity check to see that our model tagged it with a score of 0.99.

__<div align="center">Text classified as having the lowest legalization sentiment__

> (new york, new york) – on the eve of the 2016 united nations general assembly special session (ungass) on the world drug problem, world leaders and activists have signed a letter to un secretary general ban ki-moon urging him to set the stage “for real reform of global drug control policy.” the unprecedented list of signatories includes a range of people from senators elizabeth warren, cory booker, richard durbin and bernie sanders, to former president jimmy carter, former secretaries of state hillary clinton and george shultz, businessmen warren buffett, richard branson, eli broad and mo ibrahim, actors michael douglas and woody harrelson, super bowl champion tom brady, singers john legend, macklemore and mary j. blige, activists reverend jesse jackson, gloria steinem and michelle alexander, as well as distinguished legislators, cabinet ministers, and former un officials. “the drug control regime that emerged during the last century,” the letter says, “has proven disastrous for global health, security and human rights. focused overwhelmingly on criminalization and punishment, it created a vast illicit market that has enriched criminal organizations, corrupted governments, triggered explosive violence, distorted economic markets and undermined basic moral values. “governments devoted disproportionate resources to repression at the expense of efforts to better the human condition. tens of millions of people, mostly poor and racial and ethnic minorities, were incarcerated, mostly for low-level and non-violent drug law violations, with little if any benefit to public security. problematic drug use and hiv/aids, hepatitis and other infectious diseases spread rapidly as prohibitionist laws, agencies and attitudes impeded harm reduction and other effective health policies. “humankind cannot afford a 21st century drug policy as ineffective and counter-productive as the last century’s.” “the influence and diversity of the leaders who signed this letter is unprecedented,” said ethan nadelmann, executive director of the drug policy alliance, which orchestrated the initiative in collaboration with dozens of allied organizations and individuals around the world.

__Explanation of model tagging (low legalization sentiment)__: As another sanity check, we can also take a look at one of the texts that was tagged by our Random Forest classifier as having the lowest (i.e. most negative) legalization sentiment. Our model predicted a score of 0.11 for the above text (the lowest model-tagged legalization sentiment), which makes sense considering this text discusses the drug control regime that has predominated over the last century, and characterizes the war on drugs as "disastrous for global health, security, and human rights." It also notes that policymakers who assume a harsh stance on drugs have "focused overwhelmingly on criminalization and punishment, creating a vast illicit market that has enriched criminal organizations, corrupted governments, triggered explosive violence, distorted economic markets and undermined basic moral values." As such, while this text does discuss an upcoming UN general assembly session aimed to rectify the missteps of the war on drugs, it focuses mostly on how the war on drugs has failed in many ways, so it makes sense that our model tagged it as having a very low legalization sentiment score.

__Total texts classified as having a positive legalization sentiment:__ 420<br>
__Total texts classified as having a negative legalization sentiment:__ 465

## 6) PREDICTING LEGALIZATION SENTIMENT BASED ON MORTALITY DEMOGRAPHICS

As a reminder, our research question is as follows: __Do the demographics of drug-related mortalities (i.e. race, age, gender) in the U.S. impact respective messaging surrounding the legalization of drugs in press releases and newspapers?__

To address our research question, we will run a predictive model that aims to predict whether the body of text published in each state-year combination is generally positive toward drugs or negative toward drugs, based on the demographics of mortalities in that state and year. Thus, the goal of our model is two-fold. First, we want to see if it is possible to predict changing sentiments around drugs based on demographics of mortalities with high accuracy. However, it's not super interesting to say, "Hey, we were able to successfully predict changing sentiments." So, the interesting part of our model is interpreting the coefficients of our features. By examining the coefficients of our model, we will be able to tell how increases/decreases in mortalities in specific demographics affect sentiments expressed in a given year and state. For example, does a change in drug-related mortalities of White individuals or Black individuals have a greater affect on sentiments expressed by lawmakers and media outlets? Do mortalities caused by heroin or cocaine have a greater impact on drug-related sentiments? Answering questions like these is what we hope to achieve with our model.

As discussed previously, there are many different binary classification models we could use to predict legalization sentiments based on mortality demographics. However, unlike when we were modeling text data and had to hand tag a sample of texts for our training data, we now have access to the true output classes. This means that we can run different models and compare the performance of each. Below, you will see that we compare the results of 4 different classification techniques to come up with our best classification model: Logistic Regression, K-Nearest Neighbors, Random Forest, and SVM (Support-Vector Machine). After merging and cleaning the data a bit, we ran these 4 models with demographics of mortalities as our features and legalization sentiments as our outputs. Here are the results:

### -- LOGISTIC REGRESSION --

![alt text](Images/log_cm.png)

![alt text](Images/log_roc.png)

### -- K-NEAREST NEIGHBORS --

![alt text](Images/neighbors_cm.png)

![alt text](Images/neighbors_roc.png)

### -- RANDOM FOREST --

![alt text](Images/forest_cm.png)

![alt text](Images/forest_roc.png)

### -- SUPPORT VECTOR CLASSIFICATION --

![alt text](Images/svc_cm.png)

![alt text](Images/svc_roc.png)

### -- MODEL COMPARISON --

![alt text](Images/model_comparisons.png)

In general, higher levels of accuracy, precision, recall, and AUC represent a better model. The above bar chart shows accuracy, precision, recall, and AUC for our 4 models on the test data. With the highest accuracy, precision, recall, and AUC, it looks like logistic regression is our best model!

## 6) INTERPRETING THE COEFFICIENTS OF OUR LOGISTIC REGRESSION MODEL

In terms of explaining our results, we can first say that we were able to predict changes in drug-related sentiments expressed by policymakers and media outlets based on the demographics of mortalities with above 65% accuracy, above 63% precision, and above 67% recall. It's important to note that this performance doesn't necessarily confirm that there is, in fact, a concrete correlation. However, it does suggest that there may be a relationship between 1) the characteristics of individuals that fall victim to drug-related mortalities and 2) drug-related legalization sentiments expressed in legal/news coverage. This is significant in and of itself because it suggests that the who/what/when/where/why of drug-related mortalities may affect the conversations that occur surrounding drugs in legal circles as well as media circles.

However, as mentioned earlier, the really interesting part of our model is interpreting the coefficients of our features in our best logistic regression model. In a logistic regression model, the coefficient of a feature represents the increase (positive coefficient) or decrease (negative coefficient) in the log-odds of belonging to class 1 with a 1 unit increase of that feature. In the context of our model, a positive coefficient of a demographic feature means the log-odds of text being pro-legalization/anti-criminalization increases with a 1 unit increase in deaths in that demographic group (i.e. direct relationship). And a negative coefficient of a demographic feature means the log-odds of text being pro-legalization/anti-criminalization decreases with a 1 unit increase in deaths in that demographic group (i.e. inverse relationship). But what do log-odds actually mean? Log-odds is technically defined as the logarithm of the odds of success, where success is defined as belonging to class 1. The issue with interepreting log-odds is that they are not as straightforward as interpreting probabilities, but are necessary to use when performing a binary classification task. More specifically, while higher log-odds generally mean higher probabilities, it’s hard to say by how much because the change in probability that corresponds to a given change in log-odds depends on where we start on the probability vs. log-odds graph. This is an issue in terms of interepreting our results because it makes it difficult to make inter-demographic comparisons. As a result, to achieve more interepretable results, we wanted to create a probability metric to examine how a 10 unit increse in, for example, deaths of Black individuals, increases or decreses the probability of observing pro-legalization/anti-criminalization sentiments.

In terms of how we actually converted log-odds into interepretable probabilities, the log_odds_to_probabilities function below converts the intercepts for our features in our logistic regression model into the probability of observing class 1-tagged text (where class 1 is defined as pro-legalization/anti-discrimination) given a 10 unit increase in deaths in that feature category (i.e. deaths in the 25-34 year age group, deaths of White individuals, etc.) As mentioned, a given change in log-odds depends on where we start on the probability vs. log-odds graph, so we had to choose the location of the 10 unit increase (i.e. 0 to 10 deaths vs. 10 to 20 deaths). We decided to display the probabilities going from 0 to 10 deaths for ease of intrepretation. The math of converting from log-odds to probabilities is the following:

Converting probability when # of deaths in demographic of interest is 0:
 - logoddds = intercept + (0 * coefficient) = intercept
 - odds = np.exp(logodds) = np.exp(intercept)
 - probability_0_deaths = odds / (1 + odds)
 
Converting probability when # of deaths in demographic of interest is 10:
 - logoddds = intercept + (10 * coefficient) = intercept + coefficient
 - odds = np.exp(logodds) = np.exp(intercept + coefficient)
 - probability_10_deaths = odds / (1 + odds)
 
Finding change in probability of observing pro-legalization text with a 10 unit increase in deaths in demographic of interest (from 0 to 10):
 - probability_10_deaths - probabillity_0_deaths
 
Thus, the "change in probability" column in the following tables represents the change in probability of observing pro-legalization/anti-criminalization sentiments with a 10-unit increase in deaths in each demographic category. Please note that while we attempt to hypothesize about what these changes in probabilities mean about the relationships between mortality demographics and text sentiments, we understand that correlation does not imply causation.

### -- CAUSE OF DEATH ANALYSIS --

| Feature (cause of death) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| Cannabis (derivatives) | -0.006888 | -0.017215 |
| Methadone | -0.007583 | -0.018951 |
| Cocaine | -0.008206 | -0.020507 |
| Heroin | -0.009072 | -0.022669 |
| Other opioids | -0.011566 | -0.028891 |

Looking at the changes in probabilities, we see that all 5 drug types have negative probabilities. This means that according to our model, increases in drug-related deaths across all 5 drug types result in a lower probability of observing pro-legalization sentiments. That being said, we can make some comparisons based on the magnitudes of probabilities. Cannabis has the least negative change in probability of -0.017. This means that according to our model, a 10-unit increase in cannabis deaths results in a 1.7% decrease in observing Class 1 (pro-legalization/anti-criminalization) texts. It makes sense that cannabis-related deaths have the least negative effect on drug-related sentiments because it is the most widely used and least harmul of the 5 drugs. The drug with the most negative change in probability is opioids. With a 10-unit increase in opioid-related deaths, our model suggests that the probability of observing pro-legalization sentiments decreases by 2.9%. Considering the opioid epidemic is a big topic of conversation in the U.S.—opioid overdoses accounted for more than 42,000 deaths in 2016—it makes sense that opioid-related deaths have the strongest negative effect on legalization sentiments. 

### -- WEEKDAY ANALYSIS --

| Feature (weekday) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| Sunday | 0.025228 | 0.062696 |
| Monday | 0.023413 | 0.058230 |
| Wednesday | 0.023413 | 0.040461 |
| Friday | 0.010189 | 0.025443 |
| Thursday | -0.004135 | -0.010336 |
| Saturday | -0.004614 | -0.011534 |
| Tuesday | -0.006120 | -0.015298 |

The above table suggests that increases in drug-related deaths on Sunday, Monday, Wednesday, and Friday result in an increased probability of observing pro-legalization sentiments, while increases in drug-related deaths on Thursday, Saturday, and Tuesday result in a decreased probability of observing pro-legalization sentiments. While the day of the week on which deaths occurred is perhaps the least insightful of our demographic categories, it's still interesting to think about why this may be the case. For example, for Saturday, we see that a 10-unit increase in drug-related deaths occuring on a Saturday results in a 1.1% decrease in the probability of observing pro-legalization sentiments. One reason why we could be seeing this is that many drugs in our dataset are frequently used as party drugs, such as cocaine and opioids, so it's possible that the population of individuals who fall victim to drug use on Saturdays could be younger and more affluent, and thus have a greater negative effect on perceptions of lawmakers and media outlets.

### -- RACIAL GROUP ANALYSIS --

| Feature (race) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| White | 0.007731 | 0.019314 |
| Black or African American | 0.004533 | 0.011329 |
| American Indian or Alaska Native | -0.025965 | -0.064592 |
| Asian or Pacific Islander | -0.029613 | -0.073551 |

Understanding how increases in deaths across racial groups differentially affects the probability of observing pro-legalization sentiments is quite interesting. From the table above, our model suggests that a 10-unit increase in deaths of White individuals results in the highest increase in the probability of observing pro-legalization sentiments—1.9%. This means that when White individuals are afflicted by drug overdoses, moreso than any other racial group, the likelihood of observing pro-legalization/anti-criminalization sentiments expressed by policymakers and media outlets increases. This could suggest, for example, that in response to deaths of White individuals, public perceptions err on the side of treaatment rather than incarceration. At the same time, when there is a 10-unit increase in deaths of Black individuals, we observe a change in the likelihood of observing pro-legalizations sentiments that is about half that of White individuals (1.1% vs. 1.9%). This means that deaths of White individuals have a greater positive impact on legalization sentiments than deaths of Black individuals. This phenomenon of racially disparate treatment of the drug epidemic has been covered widely. For example, there was a research study published in 2017 in which the authors performed a content analysis of press articles related to drug use, and found a consistent contrast between criminalized urban black and Latino heroin injectors with sympathetic portrayals of suburban white prescription opioid users. The authors note, "Media coverage of the suburban and rural opioid “epidemic” of the 2000s helped draw a symbolic, and then legal, distinction between (urban) heroin addiction and (suburban and rural) prescription opioid addiction that is reminiscent of the legal distinction between crack cocaine and powder cocaine of the 1980s and 90s" ([Source](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5121004/)). Our model confirms findings such as these in that deaths of White individuals have the greatest positive effect on legalization sentiments expressed in news articles and press releases, while deaths of Black, American Indian, and Asian individuals have progressively lower effect.

### -- PLACE OF DEATH ANALYSIS --

| Feature (place of death) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| Other | -0.010381 | -0.014903 |
| Medical facility - dead on arrival | -0.011863 | -0.029631 |
| Medical facility - outpatient or ER | -0.012159 | -0.030370 |
| Decedent's home | -0.012424 | -0.031029 |
| Medical facility - inpatient | -0.014903 | -0.037203 |

The most interesting takeaway from the above table is that drug-related deaths occuring in a decedent's home or an inpatient medical facility have the greatest negative effect on legalization sentiments. According to our model, a 10-unit increase in deaths occuring in a decedent's home results in a 3.1% lower probability of observing pro-legalization/anti-criminalization sentiments. Additionally, a 10-unit increase in deaths occuring in an inpatient medical facility results in a 3.7% lower probability of observing pro-legalization/anti-criminalization sentiments (or correspondingly an increased probability of observing anti-legalization/pro-criminalization sentiments). If we think about why, for example, deaths occurring in an inpatient medical facility have such a negative effect on legalization sentiments, it could be because these deaths occur when individuals seek out help. The implication could be that legalization- or treatment-based initiatives are viewed as unsatisfactory, and as a result when deaths occur in an inpatient medical facility, lawmakers take a more harsh stance on drugs.

### -- INJURY INTENT ANALYSIS --

| Feature (injury intent) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| Suicide | 0.008130 | 0.020309 |
| Unintentional | 0.006249 | 0.015615 |
| Undetermined | 0.004283 | 0.010705 |
| Non-injury, no intent classified | 0.004283 | -0.038412 |

Looking at the above table, we see that suicide-related drug overdoses result in the most positive affect on legalization sentiments. A 10-unit increase in suicide-related deaths results in a 2.0% increase in the probability of observing pro-legalization/anti-criminalization sentiments in press releases and news articles. If we think about the general approah to suicide prevention, it primarily involves mental health treatment- and prevention-based programs. This could explain why we see the highest increase in the likelihood of observing pro-legalization sentiments, as pro-legalization sentiments typically involve non-violent alternatives to the war on drugs such as treatment-based programs (i.e. safe consumption spaces). So, when there is an increase in suicide-related drug mortalities, it is possible that lawmakers are more likely to assume a treatment-based stance on drug use, which is why we see the highest probability.

### -- AGE GROUP ANALYSIS --

| Feature (age) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| 25-34 years | 0.008445 | 0.021096 |
| 65-74 years | 0.004811 | 0.012023 |
| 55-64 years | 0.004787 | 0.011965 |
| 45-54 years | 0.003161 | 0.007902 |
| 15-24 years | -0.008906 | -0.022255 |
| 75-84 years | -0.010115 | -0.025271 |
| 35-44 years | -0.017067 | -0.042583 |
| 85+ years | -0.028431 | -0.070653 |

The changes in probabilities for different age groups shows that deaths in the 25-34, 45-54, 55-64, and 65-74 year age groups generally increase the probability of observing pro-legalization sentiments, while deaths in the 15-24, 35-44, 75-84, and 85+ year age groups generally decrease the probability of observing pro-legalization sentiments. This segmentation is interesting in that middle-aged groups tend to have a more positive effect on the viewpoints assumed by policymakers, while younger and older age groups tend to have a more negative effect on the viewpoints assumed by policymakers. This could be the case because middle-aged individuals are more likely to use drugs recreationally, which may lead policymakers and media outlets to assume a treatment-based rather than incarceration-based (i.e. more positive) stance on drug use. On the other hand, we see that a 10-unit increase in deaths in the youngest age group (15-24 years) lowers the probability of observing pro-legalization sentiments by 2.2%. One reason why this could be the case is that 15-24 year-olds who pass away from drug use most likely are affected by external factors, so the presumed "best" solution to youth drug use in the eyes of policymakers could simply be to take a harsher stance on drugs.

### -- GENDER ANALYSIS --

| Feature (gender) | Logistic regression coefficient | Change in probability |
| ----------- | ----------- | ----------- |
| Female | 0.003911 | 0.009775 |
| Male | -0.000638 | -0.001594 |

The changes in probabilities we observe for males and females is quite interesting. Our model suggests that a 10-unit increase in female deaths results in a 1.0% increase in the probability of observing pro-legalization sentiments, while a 10-unit increase in male deaths results in a 0.1% decrease in the probability of observing pro-legalization sentiments. This suggests that deaths of females may cause a more lax (anti-criminalization/pro-treatment) stance on drugs, while deaths of males may cause a more harsh (pro-crriminalization/anti-treatment) stance on drugs. In terms of why this could be the case, it could be because of the underlying differences in drug use between males and females. According to the Addiction Center, men are: more likely to become addicts, more likely to abuse substances due to peer pressure, and more likely to stabilize substance abuse than women. Women, on the other hand, are: more likely to transition from substance abuse to substance dependence, more likely to self-medicate with illicit substances, and more likely to suffer substance abuse sidde effects than men. As such, when the number of females who die from drug overdoses increases, policymakers and media outlets may be more likely to take a more positive, treatment-based approach to drug use as a function of these differences.

## 7) CONCLUSIONS

The implications of this research are many. First, the fact that we were able to predict changes in drug-related sentiments based on the demographics of mortalities with above 65% accuracy, above 63% precision, and above 67% recall suggests that policymakers may be influenced (either positively or negatively) by the demographics of individuals afflicted by drug-related mortalities. In theory, policy should represent the "best" solution to the problem at hand for all demographics, and should not change based on the surface-level characteristics of afflicted populations. That being said, our model involves predicting intangible qualitative sentiments from tangible, real-world quantitative numbers, so we recognize that there may not be a direct correlation. Our analyses of the changes in probabilities of observing pro-legalization sentiments also have important implications. For example, in the context of our analysis of the effects of deaths of White vs. Black individuals, it's important to ask why we might observe more pro-legalization sentiments in the wake of deaths of White individuals. Does this reflect a biased attitude of policymakers that views White drug users as victims and Black drug users as criminals? Again, we have to qualify questions like this by acknowledging that a simple machine learning algorithm does not capture the full scale of intricacy and complexity in the real world.

In terms of directions for future research, it would be interesting to see this analysis carried out over a long time period. The history of the drug epidemic has been extremely tumultuous in the U.S. For example, the crack/cocaine epidemic of the 1980s had particularly devastating effects in African American communities, causing increases in addictions, deaths, and drug-related crimes. How did the crack/cocaine epidemic affect the attitudes of policymakers in the 1980s, and were there lasting effects through to the end of the 1900s? Considering we grouped our data by unique state and year combinations, it would also be very interesting to consider how the majority political party affiliation of each state affected legalization sentiments over time. Have Democrat-leaning states always assumed a more treatment-based approach to the drug epidemic in the U.S.? Finally, future analyses in this field could explore a wider array of demographics. In addition to relatively surface-level demographics such as age and race, looking into the effects of drug-related deaths based on income level, urbanization, education level, etc. could produce noteworthy insights. That being said, we believe we've performed a unique analysis of the drug epidemic in the U.S. by merging real-world mortality data with qualitative text data to understand how the demographics of mortalities affect sentiments expressed by policymakers and media outlets.
