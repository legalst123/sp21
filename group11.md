---
layout: page
title: Group 11
nav_order: 16
description:
---
# Web Presentation

Jasper Zhou, Muskaan Soti, Sarah Lambert, Rishi Upadhyay

### Question & Motivation

  Our group wanted to perform sentiment analysis on Supreme Court nominee hearings for several key reasons. Recent literature shows that Supreme Court nominations have become increasingly politicized over the past few years. Due to this, our group was curious to learn whether we could measure the rhetoric in Supreme Court nomination hearings to see if polarity and subjectivity was indeed changing. We were also motivated to apply the Moral Foundations Theory dictionary on our text and observe what moral foundations are relevant in Supreme Court nominee hearings, and whether those foundations have changed depending on the nominee and across party lines. 

  This is an important question because the Supreme Court’s purpose is to be an independent branch of the government that can effectively make influential decisions about the law while being free from party, media, and the public’s influence. If rhetoric surrounding Supreme Court nominees is becoming increasingly polarized and subjective, this could indicate a decline in the quality of the Supreme Court and the nomination process. Sentiment analysis on Senate hearings on Supreme Court nominees can give us insight on the integrity of the Supreme Court and the nomination process. Also, this question could potentially help us make associations with certain types of Supreme Court nominees and the rhetoric in discussions about them. For example, sentiment analysis can tell us the specific kinds of rhetoric or moral foundations associated with conservative and  progressive candidates and allow us to compare the findings. 

  Another important question we wanted to explore is the usefulness of natural language processing (NLP) in the field of law and government as a whole. Analyzing nominee hearings or other legal texts requires a significant amount of labor and time since legal texts tend to be very long and comprehensive. The integration of NLP in the legal field could automate parts of the analysis process on legal text and help those in the legal field make important conclusions. NLP could improve efficiency, save time and provide a methodical approach to analyzing legal text. Additionally, NLP could also help in terms of equity by making legal texts more understandable and accessible to the public. Therefore, another purpose of this project is to gauge the reliability of NLP techniques in the legal realm and determine whether it is a promising tool for analyzing future legal text. 


## Data

### Data Acquisition & Cleaning

We initially got all our data as .xml files from the senate.gov website. We used Beautiful Soup to parse these files like we were taught in class. Specifically, we used Beautiful Soup to remove unecessary parts and extract speaker names. After this, we were able to create a dataframe per justice that had a row for each thing spoken during the hearing. 

We simultaneously acquired a dataset of all past and current Senators' parties, states, years in service, and more from Wikipedia. We cleaned this data using pandas and then combined it with the other table to end up with a table that had 1 row per thing spoken along with information about the speaker's party, state, age and more.

Here is an example of the dataframe:

![df.png](./images/example_df.png)

## Exploratory Data Analysis

Most our dataset was text-based, so we decided that the one of the best ways to understand and explore the dataset at first was through word clouds. We created word clouds for every justice that allowed us to easily see what sort of words were used often, whether there were similarities across hearings or not, and more. 

As an example, here is a word cloud for the recent hearing of Neil Gorsuch:

![gorsuch.png](./images/gorsuch_cloud.png)

There are a few interesting words: Marshall, little, and Length all raise questions about the dataset that will be interesting to answer. 

We also created word clouds for older hearings, in this case the hearing of Clarence Thomas:

![thomas.png](./images/thomas_cloud.png)

This hearing was unusually long and gave us a different insight into the words and patterns behind Supreme Court Hearings.

## Modeling

### Sentiment Analysis

One major analysis we did was Sentiment Analysis. We did this in order to track the polarity and subjectivity of discourse in Supreme Court hearings over time. 

The polarity score is measured from a -1 to 1. -1 exemplifies a highly negative text and 1 represents a highly positive text. The subjectivity score is measured from a 0 to 1, where 0 is objective and 1 is very subjective.

Using NLTK, TextBlob, and Seaborn, we were able to compute the average polarity for all hearings we had access to:

![avg_p.png](./images/avg_polarity.png)

We also computed the average subjectivity:

![avg_s.png](./images/avg_subj.png)

Overall, this data was not very interesting. Polarity slightly increased and Subjectivity was nearly constant, so we decided to look into the differences across party. Using similar techniques as before but split using the 'Party' column of our dataset, we were able to generate these plots:

![apd.png](./images/abs_p_diff.png)

![asd.png](./images/abs_s_diff.png)

### Moral Foundations Theory

The goal of this analysis was to track the moral attributes given to each nominee. 

Moral Foundations Theory (MFT) proposes that there are five main moral foundations in society: Care/Harm, Fairness/Cheating, Loyalty/Betrayal, Authority/Subversion, and Sanctity/Degradation. MFT examines how moral foundations can structure different cultures and suggests a relation between political ideology and an individual’s valued moral foundations. Typically, liberals esteem harm/care and fairness/reciprocity more than the other three foundations, while conservatives tend to value the five foundations equally. 


We created these plots for both overall attributes and difference between parties:

![mft.png](./images/MFT_avg.png)

![dmft.png](./images/DMFT_avg.png)

![rmft.png](./images/RMFT_avg.png)

## Results

### Sentiment Analysis

#### Average Polarity
In our average polarity plot, we found that the average polarity scores for the justices were relatively similar. We observed that Justice Ginsburg, Justice Sotomayor, and Justice O’Connor had average higher polarity scores signifying they had more positive reactions than the other justices. All three of these justices are women. Justice O’Connor’s high polarity score is likely because she was the first woman Supreme Court justice ever elected. Her nomination occurred in the aftermath of the women’s rights movement that achieved suffrage rights. Justice Sotomayor’s score is likely influenced by her status as the first Hispanic and Latin American Supreme Court justice, and her positive reception amongst the Senate as the “easy one” out of Obama’s nominees, evidenced by the 68-31 confirming vote. Justice Ginsburg was nominated under Clinton and confirmed in a Democratic majority Senate, and likely earned such a high polarity score due to her accomplishments with the women’s rights movement. Ginsburg also peaked in the care/harm and fairness/reciprocity MFT categories, which is informed by her work on equality and fair treatment for the sexes under the law. 

#### Polarity 
Justices O’Connor, Scalia, and Rehnquist were the three most controversial nominations. Justice O’Connor is interesting in that she rates high in both positive reactions and controversial nominations. This is likely due to the fact that the comments in her favor were very gracious and supportive because of her opportunity to be the first woman on the Supreme Court, while her nomination in and of itself still drew controversy because of that same fact. Justice Scalia is highly conservative, which informs his score. Chief Rehnquist was the last of Nixon’s nominations and was intended to be more agreeable but was still controversial given the president who nominated him to the court. He also has a sordid history with racially insensitive comments, but given the year of his nomination, those were not likely a factor in his score.

#### Subjectivity
Justice O’Connor and Justice Alito had the highest subjectivity scores. A high subjectivity score signifies how opinionated the hearings around a certain justice were, and whether they were more neutral or biased. It makes sense why Justice O’Connor had a high subjectivity score considering she was the first woman justice to be nominated therefore there were probably strong opinions on her nomination. Justice Alito's highly conservative viewpoints could explain why his subjectivity score was high too. 

### Moral Foundations Theory

We noticed that Justice Rehnquist had a high loyalty/betrayal score in the moral foundations theory plot. We contend that this may be attributed to his controversial past. Justice Rehnquist had made racist and anti-semitic remarks, and reaffirmed the “separate but equal” stance decided in Plessy v. Ferguson. His discriminatory remarks and actions led people to question his loyalty to the country since he did not appear as an advocate for all citizens. His prejudiced remarks were brought up in the Senate Nominee hearings which could explain why his loyalty/betrayal score is higher than the other justices. 

In terms of MFT as a whole, authority/subversion was considered to be far more important than the other four categories. Given the nature of the Supreme Court and the characteristics needed to be a successful judge, it is no surprise that authority was considered to be an important factor in one’s fitness for office. Amongst Democrats and Republicans, there was not much difference in their MFT scores, with both parties considering authority/subversion the most critical, and considering the other four categories to be about the same. Sanctity/Degradation was the lowest of the five, but that is probably due the fact that this foundation centers around contempt and religious beliefs, which is not as relevant to the nomination process.

## Conclusion

Our original assumption was that based on the politicization of the Supreme Court, we would expect the polarity and subjectivity score to increase over the past years. We also expected to see a clear difference in the moral foundations associated with Senate hearings on conservative nominees and more progressive nominees. However, our results showed that the difference in polarity and subjectivity score between candidates is generally not too drastic. While there are certainly some nominees with higher polarity and subjectivity scores, the trend in polarity and subjectivity over time is not clearly upward or downward. Instead, it seems to be contingent on the social factors affecting the nominee and their hearings. Additionally, the moral foundations associated with Senate hearings was quite similar for both progressive and conservative Supreme Court nominees. In general, the Senate hearings on both progressive and conservative nominees yielded a notable weight on the “Authority/Subversion” moral foundation than the other moral foundations. The general hypothesis is that the moral foundations associated with progressive and conservatives should be different, but this was not what we observed. 

Our findings were that there are differences in polarity for certain candidates - especially when the candidate is unique or unconventional in some way. As we noted above, the rhetoric around women Supreme Court nominees generally was positive because the Supreme Court was dominated by men until the last century. Additionally, the polarity and subjectivity was high with more controversial candidates such as Justice Rehnquist and Justice Alito, both of whom had very conservative beliefs that bordered on  outright discriminatory. Therefore, while there are not distinct differences in polarity, subjectivity, and moral foundations over time, there certainly are differences based on each individual candidate and the circumstances surrounding their nomination. 

While these findings were not exactly what we anticipated, our methods definitely did serve our goals in this project. Even though we did not witness a clear trend in increased polarity over time, we were able to make associations with certain attributes of candidates and their polarity, subjectivity and moral foundations scores. It was refreshing and more intriguing to find conclusions that we did not expect. Knowing that rhetoric around progressive and conservative nominees are usually associated with the same moral foundations tells us that fundamentally Supreme Court justices are quite similar. This finding challenges the general stereotype that progressive justices and conservative justices are morally very distinct. Also, discovering that individual attributes of candidates—whether they are an unorthodox candidate, historic in any way (first in their ethnicity/gender to be nominated)—is highly related to their polarity score is a helpful finding that can be utilized for future predictive tools in the Supreme Court nomination process.

