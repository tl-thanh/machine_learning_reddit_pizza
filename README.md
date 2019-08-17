# This repository holds the python code, *Scikit-learn*, for the analysis of Reddit - Random Acts of Pizza.

Team members: Andy Cui, Eli Hanna, Thanh Le, Edina Tankovic

**Prologue**
Reddit is an online community/forum created in 2005. On reddit, users can anonymously post messages, comment on said posts, and upvote and downvote posts and comments. Reddit also has a concept of a subreddit, which are similar to forum boards and specialize in topics. 
Our project focuses on a specific subreddit: /r/Random_Acts_Of_Pizza/. In this subreddit, users can post a request for pizza, explaining their request in the post text. Other users can then respond, and send a pizza to the requester in an act of generosity. 
We believe that there is a lot to be gleaned about human behavior, specifically on what it takes to be persuasive enough to get a random stranger to be generous enough to deliver free pizza to you. Our main focus is on predicting what it takes to get free pizza, but along the way we explore other latent factors of this active and interesting online community.

**Introduction/Setting Up**
Our most important work in this area was defining new features to be used for modeling and understanding our problem. We added four “sentiment” features that allow us to get a feel for how friendly, invested, and positive a post is. We also added word count features for the length of the main body of the post and the title. Finally, we broke the timestamp down into separate features indicating the local hour, utc hour, and day of the week that a post was made. 

**Exploratory Data Analysis**
What became obvious very early on was that requesting pizza had actually a low probability of being granted (<25%). We examined the correlation between variables. Apart from correlation between 'upvotes_minus_downvotes <->'upvotes_plus_downvotes', 'total_comments'<->'total_subreddits' and 'account_age'<->'total_subreddits' nothing stood out. We plotted number of words per post, number of words per title, how many posts requestor had up to that point, upvotes minus downvotes, smile emoticon present pizza success, sad emoticon present pizza success, url link present pizza success, day of week local and time of day local pizza success.
 
**Dimensionality Reduction and Clustering**
In this section we aim to better understand the clusters in our data and hopefully provide a semantic description for the various clusters. We first featurized each post by using a CountVectorizer, then used TruncatedSVD to do dimensionality reduction to 80 dimensions. This explained  73% of the variance of the data. We then ran k-means with 8 clusters hoping to be able to explain each cluster semantically. Our conclusion was that the last cluster is clearly distinct and contains long posts authored by very active users. These users post in multiple other subreddits and their posts gain a lot of voting activity as well. We believe this cluster would be very interesting to focus on for a targeted analysis. Finally, we provide our cluster values as a feature for the supervised prediction model. 

**Supervised Prediction Model**
Our final model of choice for supervised prediction was the Random Forest Classifier. This is because Random Forests deal well with features that have very different distributions, which we found was the case in our EDA histograms. Additionally, we can mitigate overfitting by using a forest as opposed to a single tree. 
One issue that we had to overcome while implementing this model was dealing with the fact that our data was unbalanced (~75% of our test data failed to receive pizza). As a result of this, our Random Forest Classifier originally achieved an AUC score of roughly .75 simply by always predicting False. We countered this behavior by setting the class_weight parameter to “balanced” and by limiting the options in the grid search. While this lowered our AUC, it was successful in achieving more balanced predictions. Despite the lower AUC score, we believe this provides a more useful representation of reality.

**Conclusion**
We found these 5 features are the keys to potential success.  

Word count of the main body of the post
Gross votes
Net votes received
Number of total comments in RAOP
Age of the account

We suspect these are good measures because readers infer wordy requests as more sincere since the requester put time and effort in the request.   In addition, having an older account with activity in the community may lend credibility to the requester. The votes and comments are measures of how many readers engaged with the requests; increased engagement may lead to improved chances of success.

However, given the complexity of human behaviors, we are unable to prescribe a specific formula to guarantee a user will receive a pizza. We can recommend users to put time and effort into writing an engaging request and build rapport in the subreddit to improve their chances. 
