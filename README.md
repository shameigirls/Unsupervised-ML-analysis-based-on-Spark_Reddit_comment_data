# Unsupervised-ML-analysis-based-on-Spark_Reddit_comment_data
This is an unsupervised ML analysis based on Spark.

Code: Redditcomments_ML_massive_data.ipynb

Methods:
For the exploratory data analysis, I read the LZO file, got the schema from the json file, and then converted the LZO file to a data frame using the schema. Then I registered 2 data frames and union them together into 1 data frame. (I tried to include all 4 of them, but it took too long to run). I checked the schema of the merged dataset, printed the first few rows of the dataset, counted the number of records, checked the NA for column” score”. To visualize the score column, I drew histograms for it by taking 10000 samples from all. I also drew boxplots for other interested numeric variables including "collapsed", "controversiality", "distinguished", "gilded", "is_submitter", "send_replies", "no_follow", "stickied" and "subreddit_type", so as to see the distribution of those features. The plots show that there are very little difference/ imbalance labels for distinguished and subreddit_type, thus those features will not be considered in to modeling. Stickied is also among them, which has very distinct variance by glance, but it could be an important feature influencing the score of the comments, so I decided to join it to the model next step.

Tools for analysis: I used Spark as the main tool. Three models of logistic regression, random forest ang GBT.

Modeling: I used score as the label for SparkML models, as it is an important index and characteristic of the comment, also it may be closely related to other features, thus make it an interesting feature to explore. To classify the score, I checked the max, min, quantile and mean of the score value. By combining the exploration and the plotting of the score, I decided to set 4 as the divider point for “low score” and “high score”. For one reason, 4 as the cut point would not make the two groups has a lot difference in counts. For another, the mean is 9 and the median is 2, it makes much more sense to use 4 for dividing considering the distribution and people’s common intuition for high and low score. Before modeling, I transformed the column  “gilded” and “controversiality” using OneHotEncoder so they are categorical variables. I then split data into train, test and predict datasets, created a feature vector by combining all features together using the vectorAssembler method, build a Logistic regression model using the LogisticRegression estimator and a random forest model using the RandomForestClassifier estimator. I built the pipelines consisting of an estimator. I trained and fit the logistic regression model, random forest model and GBT model respectively, evaluate the accuracy of all three of the models by BinaryClassificationEvaluator, MulticlassClassificationEvaluator and GBTClassifier estimator.

Concept: I tried to run this unsupervised ML models, in order to predict the scope of score of a certain comment, based on whether the comment is distinguished by the moderators, whether the submission is set as sticky in the subreddit, number of times this comment received Reddit gold, and number that indicates whether the comment is controversial, etc.

Result: It turns out from the predictions and the accuracy check, that the performance of the three models is very much similar in this situation. Logistic regression shows a slightly higher accuracy than the others. However, the overall result of evaluations of the models shows all of them could be useful for predicting a high or low score of a comment.

Future work: I would probably use more models to train and fit the data in order to get the best-fitting one, adding more features and use natural language processing to add the contents/body of the comments as one important feature. Besides, I could also use other tools like pig and Hive to do queries of the datasets and explore more about it.




