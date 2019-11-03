# Relax Inc Take home Challenge

### Take home challenge summary.
In this take-home challenge, I was given two data files, takehome_users.csv and takehome_user_engagement.csv, and asked to determine which features predict future user adoption. The first data file contains information on 12,000 users, while the second file contains user login information. I explored the datasets and cleaned the data of null values. Using the login information, I identified adopted users as those who logged in on three separate days in a 7-day period. I then created a random forest model and trained it on the data to predict user adoption. I examined the features of the model, looking into the features with highest importance as these should have the highest predictive power.

### Feature engineering

After data cleaning, I saved my cleaned data as 'userdata_with_active_label.csv'. This dataset has the following features: 

 name: the user's name
 
 object_id: the user's id
 
 email: email address
 
 creation_source: how their account was created. This takes on one of 5 values (PERSONAL_PROJECTS, GUEST_INVITE, ORG_INVITE, SIGNUP, SIGNUP_GOOGLE_AUTH)
 
 creation_time: then they created their account
 
 last_session_creation_time: unix timestamp of last login
 
 opted_in_to_mailing_list: whether they have opted into receiving marketing emails
 
 enabled_for_marketing_drip: whether they are on the marketing email drip
 
 org_id: user group they belong to
 
 invited_by_user_id: which user invited them to join (if   applicable).
 
 is_engaged: 1 if the user has loggen in on three separate days in a 7-day period, 0 otherwise.
 
 The name, object_id and email features were removed from the model. None of these should have any predictive power over user engagement, or would not be useful if they did. The target variable for the model to predict is is_engaged.


### Model results

I trained a random forest model on this dataset and performed a grid search with varying estimators and max depth, using a 5-fold cross-validation and taking the average of the five test scores to find the best model. I reached ~96% accuracy, which seemed sufficient to think that my model was accurately predicting user adoption on this dataset. I then pulled out the feature importances of the features in this dataset (see below).

![Feature importances, sorted.](graphs/Feature_importances_of_Random_Forest.png?raw=true "Feature importances")

As seen on the graph, the most predictive features were the creation_time and creation_source features. I then explored these features to see if I could find the trends in this data that my model had identified.

The creation_time field is a date identifying when the user first created an account. I sorted the accounts by creation date grouped the accounts into batches of 500, such that the firest group would have the 500 oldest accounts and so on. I then plotted the percent of users in each group who had become 'adopted' users and fit a linear regression to look for a simple trend. As seen below, I found that the average user adoption has gone up over time; newer users are more likely to adopt the product.

![% user adoption over time.](graphs/engage_pct_over_time.png?raw=true "Percent user adoption over time")

I also looked into the creation_source feature to see if there was a meaningful difference in how adopted users found this produce vs unadopted users. As seen below, the pie charts for the two groups look very similar but there are meaningful differences; for example, 22% of adopted users were invited to create counts as gudest members of an organization, vs. 18% for unadopted users.

| Adopted users  | Unadopted users |
| -------------  | -------------   |
| ![Adopted user source breakdown.](graphs/engaged_user_source.png?raw=true "Adopted user source breakdown")  | ![Unadopted user source breakdown.](graphs/unengaged_user_source.png?raw=true "Unadopted user source breakdown")   |
