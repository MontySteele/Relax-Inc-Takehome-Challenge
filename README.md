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
