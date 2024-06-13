# Waze
## Predicting What made users churned from apps

# Overview
The goal of this project was to  increase overall growth by preventing users to churn from Waze app. EDA first was made to understand relationship between variables and churn rate. Visualisations (boxplot,barplot,histogram,scatterplot) was created to examine outliers and relationship between each variables (https://public.tableau.com/views/wave_17172210637840/summary?:language=en-US&:sid=&:display_count=n&:origin=viz_share_link) Based on the averages shown, it appears that drivers who use an iPhone device to interact with the application have a higher number of drives on average. However, this difference might arise from random sampling, rather than being a true difference in the number of drives. To assess whether the difference is statistically significant a two-sample hypothesis test (t-test) was conducted to analyze the difference in the mean amount of rides between iPhone users and Android users. Regression was built to understand relationship between variables and dependent variable(label). Besides, model performance was evaluated. Logistic regression did not perform well with very low of recall score. From the regression analysis new feature(professional_driver) was ranked at top 2. So, new features were engineered to use for next machinese learning modeling. Random_forest and xgboost were picked for modeling. Each models was fitting, tuning (hyperparameters), selecting on the validation set, assessing the champion model's performance on the test set. According to the result xgboost was selected for our final model due to its highest recall among models. It recall score 17% was nearly double the score from previous logistic regression. Based on the model, km_per_hour, n_days_after_onboarding, percent_sessions_in_last_month were the Top 3 most important factors in determining whether a driver will churn or not. 


# Business Understanding
Waze, the popular navigation app, generates a significant portion of its revenue through data monetization. By leveraging the vast amount of anonymized user information it collects, Waze is able to create valuable insights and sell this data to interested parties. It involves selling this aggregated data to various entities, including government agencies, urban planners, and businesses. So its important to retent customers in order to gather more data from them.

# Data Understanding
None of the variables in the first 10 observations have missing values. Note that this does not imply the whole dataset does not have any missing values. The variables label and device are of type object; total_sessions, driven_km_drives, and duration_minutes_drives are of type float64; the rest of the variables are of type int64. There are 14,999 rows and 13 columns. The dataset has 700 missing values in the label column. Comparing summary statistics of the observations with missing retention labels with those that aren't missing any values reveals nothing remarkable. The means and standard deviations are fairly consistent between the two groups. Furthermore, features engineering was applied to create meaningful new varialbes like driven_km_drives, km_per_driving_day, drives_per_driving_day, by using median instead of mean due to outliers.

# Modeling and Evaluation
2 ways t test was conducted it shown the p-value (0.1434) is larger than the chosen significance level (5%), we fail to reject the null hypothesis which is there is no statistically significant difference in the average number of drives between drivers who use iPhones and drivers who use Androids. The binary logistic regression model has precision(0.52%), recall (0.09%), f1 (0.16%) and accuracy (0.82%) which means there are many false negative predictions and fails to capture users who will churn. 'Activity_days' was by far the most important feature with -0.106 negative coefficients to churned rate following by 'professional_driver' (-0.0015). Correlation heatmap revealed 'km_per_driving_day' have the strongest positive correlation with churn and it was the second-least-important variable. Some new features are engineered to use for modeling. The final modeling dataset contains 14,299 samples and splitting the data into train/validation/test sets (60/20/20). In this case, a 60/20/20 split would result in \~2,860 samples in the validation set and the same number in the test set. Following by Fit models and tune hyperparameters on the training set, Perform final model selection on the validation set, Assess the champion model's performance on the test set. Recall score was use as our perfomance metrics in this activity because false positive rate affected our project objective dramatically. Random_forest model was first to implement and resulted in 0.126782	of recall score which was outperformed our early logistic regression. Following by xgboost model came with 0.173468% of recall score. Validation set then was use for each model for validation purpose. No susprise XGBoost model was the final champion model for train against our test dataset (XGB:0.165680 vs RF:0.120316). Then, confusion matrics and features importance plot was created using predicted y and xgb test data to determine feature importance in who would churn or not. The plot shows that km_per_hour, n_days_after_onboarding, percent_sessions_in_last_month were the Top 3 most important factors in determining whether a driver will churn or not. Moreover, there were 2 engineered features out of top 3. XGboost model fit the data well compare to random forest model. It recall score was nearly double the score from previous logistic regression. 

Feature Importance Horizontal bar chart showing feature importance of random forest model.

# Conclusion
The median user who churned drove 698 kilometers each day they drove last month, which is ~240% the per-drive-day distance of retained users. The median churned user had a similarly disproporionate number of drives per drive day compared to retained users.
Perhaps the data in particular the sample of churned users—contains a high proportion of long-haul truckers.
In consideration of how much these users drive, it would be worthwhile to recommend to Waze that they gather more data on these super-drivers. It's possible that the reason for their driving so much is also the reason why the Waze app does not meet their specific set of needs, which may differ from the needs of a more typical driver, such as a commuter.
2 ways t test shown that drivers who use iPhone devices on average have a similar number of drives as those who use Androids. Next step is to explore what other factors influence the variation in the number of drives, and run additonal hypothesis tests to learn more about user behavior. Further, temporary changes in marketing or user interface for the Waze app may provide more data to investigate churn. The binary logstic regression has very low recall score and mediocre precesion which meant only 53% of positive predictions are correct. It makes a lot of false negative predictions and fails to capture users who will actually churn. Activity_days was the most important feature in this model. its negative correlated to churn rate. The XGBoost model made more use of many of the features than did the logistic regression model, which weighted a single feature (`activity_days`) very heavily in its final prediction. Notice that engineered features accounted for six of the top 10 features (and three of the top five). Feature engineering is often one of the best and easiest ways to boost model performance. Also, note that the important features in one model might not be the same as others model. So, should examining them carefully. The model not really useful for driving business decisions, because its not a strong enough predictor and also its poor recall score. But its can still can use to guide further exploratory efforts. Splitting the data three ways which enables testing of the champion model by itself on the test set, which gives a better estimate of future performance. Logistic regression models are easier to interpret, its only tell you if each feature is positively or negatively correlated with the target variable. XGboost model required much less data cleaning and require fewer assumptions about the underlying distributions of their predictor variables. In the case of this model, the engineered features made up over half of the top 10 most-predictive features used by the model. It could also be helpful to reconstruct the model with different combinations of predictor variables to reduce noise from unpredictive features. It would be helpful to have drive-level information for each user (such as drive times, geographic locations, etc.). It would probably also be helpful to have more granular data to know how users interact with the app. For example, how often do they report or confirm road hazard alerts? Finally, it could be helpful to know the monthly count of unique starting and ending locations each driver inputs.
