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

![image 2](images/2.png)

I then mapped each consilidated category into numbers 0 to 5, and the dataset is ready for training. 

# 2. Model Split

![image 3](images/3.png)

The dataset was split into separate training and testing sets where 80% was reserved for training, and the remaining 20% for testing. 
These sets were also fit on a TFIDF vectorizer. 

![image 4](images/4.png)

Using the feature selection in Sci-kit learn, I extracted the most correlated bigrams and unigrams in each category.
An important note here after the project is that these bigrams and unigrams doesn't necessairly describe each category because we are interested in WHY the customer cancelled, not WHAT. For example, simply "Rewrite" may go into Change in coverage needs but that doesn't tell us why a rewrite so it should go into Unknown.

# 3. Feature Engineering

![image 5](images/5.png)

In Feature Engineering, I introduced Randomized Cross Validation for Hyperparameter tuning in each machine learning algorithm. I picked 5 machine learning algorithms that were applicable for this supervisation project. Random Forest, Naive Bayes, Logistic Regression, Gradient Boosting, and SVM. Once training features were fit on the base model, I compared the best parameters with results of Grid Search CV as well; extracting the model's overall training and testing set accuracy scores. 

![image 6](images/6.png)

![image 7](images/7.png)

Overall results of each algorithm showed Random Forest having the best testing and training set accuracies and is the model I moved forward with. Gradient Boosting had the highest training set accuracy but a much lower training accuracy which indicated the model is overfitting. SVM performed the worst out of the algorithms, in the future I could have run the project again with Kernel SVM. 

Nevertheless, once aspect I could improve on is my evaluation of machine learning algorithms. In this project, I simply chose the model with best training and testing set accuracies. I will do a better job at evaluating my results with different scores such as R-Squared. 
All model's best features were then stored as pickles to be accessed later. 

# 4. Running the algorithm with comments

![image 8](images/8.png)

I connected to the Operational Data Store database using pyodbc and wrote an SQL query to return all cancellation comments. The data wrangling & cleaning process was repeated and ran against Random Forest. The following Histogram represents the model's classifications of number of comments in each category, for comments under 'Other':

![image 9](images/9.png)

Change in Coverage Needs is by far the most common reasoning as to why our customers were cancelling their policies. 

Now that we broke down Other into pre-defined consolidated categories, the same trained model was rerun on cancellation comments that weren’t Other. This step was done to show any similarities between the current dropdown choices and the categories that describe what's going on in our customer’s lives. It determines whether we should adjust our current options and asks whether customers are able to smoothly select a Cancellation Sub Type most closely relating to their provided comment. The following Histogram below represents the model's classifications of number of comments in each category, for comments in general across all dropdowns:

![image 10](images/10.png)

Analyzing cancellation comments under 'Other'
Running the model on Other cancellation comments, Change in Coverage Needs is the most common appearing category. The breakdown of these consolidated categories helps us understand the sentiment of customers who are selecting Other without going through every comment. This means that roughly 478 customers selecting Other either wanted a rewrite or did not want to renew. 215 of customers who are selecting Other had to cancel their policy to increase limits or change an effective date.

