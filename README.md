# Cancellation-comments
ML project during my Co-op at Berkshire Hathaway Specialty Insurance - Berxi
I am writing this document detailing my process and reasoning up to the deliverable, as code from the Jupyter Notebook file contains sensitive company information. 

## Problem statement:
There are a significant number of policies being cancelled under the dropdown cancellation type 'Other'. It is difficult to understand why customers are cancelling without having to read through each individual comment.

## Use case:
To perform analysis on cancellation comments in order to split them into consolidated categories which would help identify the trends that drive these cancellations. The aim is to not identify what the most prominent cancellation reasons are, but to gain information about cancellations as a whole and determine how to better serve our customer’s needs when selecting reasons from the dropdowns and mitigate issues.

This deliverable is an executable python script running a supervised machine learning model on a set of cancellation comments for categorization and analysis. A sample dataset of cancellation comments were queried from ODS Mendix table and manually labeled. The cancellation comments were made by a mix from both customers and Product Specialists.

In the re-categorizing process, we got to the consolidated categories by sorting and labeling them through 3 levels of classifications starting from lowest to a broader encapsulating category based on a different business decisions taken on each one. We also aimed to keep the number of categories to a minimum for better machine learning. Therefore, coming up with 6 pre-defined categories: Change in Coverage Needs, Major Life Event, Needed Different Eff Date or Limit, Product Fit, Test, or Unknown.

# 1. Data Cleaning
![image 1](images/1.png)

In the pre-processing stage, I cleaned the dataset with Pandas by converting all comments into lowercase, removing punctuations and pronouns, intoduced a word lemmatizer to shorten each word into it's base form, and removed stop words such as "the", "a" (NLTK Natural Language Toolkit)

I then mapped each consilidated category into numbers 0 to 5, and the dataset is ready for training. 

# 2. Model Training

The dataset was split into separate training and testing sets where 80% was reserved for training, and the remaining 20% for testing. 
These sets were also fit on a TFIDF vectorizer. 

Using the feature selection in Sci-kit learn, I extracted the most correlated bigrams and unigrams in each category.

Analyzing cancellation comments under 'Other'
Running the model on Other cancellation comments, Change in Coverage Needs is the most common appearing category. The breakdown of these consolidated categories helps us understand the sentiment of customers who are selecting Other without going through every comment. This means that roughly 478 customers selecting Other either wanted a rewrite or did not want to renew. 215 of customers who are selecting Other had to cancel their policy to increase limits or change an effective date.
