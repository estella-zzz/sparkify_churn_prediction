# Sparkify Churn Prediction with PySpark ML and AWS EMR

## Instructions
The project details are included in the blog post linked below:
[Link to the blog post](https://medium.com/@kxestella.zzz/predicting-churn-with-pyspark-ml-d65012e9ab7c?source=friends_link&sk=f69ff5ef9dbfd4f490bed130ba9b3b99)

PySpark Script (local): Sparkify_Churn_Prediction.ipynb

AWS EMR Deployment Script: Sparkify_spark_deploy.ipynb

AWS EMR Results: Sparkify_spark_deploy.html

To access full fataset in S3: "s3n://udacity-dsnd/sparkify/sparkify_event_data.json"

## Project Overview
Customer retention is critical for a thriving business. When we are enjoying the digital services with one-click-to-subscribe, companies are pulling hairs out because someone just took one click to quit. If we can identify which users are at-risk to churn, then the business can take action and potentially make them stay.

Sparkify is a music streaming service in the Udacity universe. Just like Spotify, users can choose free tier subscription with ads or paid tier without ads. We will first analyze the data on a smaller subset (128MB), then deploy the pipeline to cloud services like AWS EMR to tune selected models with the full dataset (12GB).

## Data Source
The mini dataset contains 286500 records and 225 distinct users. 

#### Data schema:

 |-- artist: string (nullable = true)
 |-- auth: string (nullable = true)
 |-- firstName: string (nullable = true)
 |-- gender: string (nullable = true)
 |-- itemInSession: long (nullable = true)
 |-- lastName: string (nullable = true)
 |-- length: double (nullable = true)
 |-- level: string (nullable = true)
 |-- location: string (nullable = true)
 |-- method: string (nullable = true)
 |-- page: string (nullable = true)
 |-- registration: long (nullable = true)
 |-- sessionId: long (nullable = true)
 |-- song: string (nullable = true)
 |-- status: long (nullable = true)
 |-- ts: long (nullable = true)
 |-- userAgent: string (nullable = true)
 |-- userId: string (nullable = true)

## Feature Selection
After exploring the dataset, 11 features have been chosen for further analysis:

#### User information
- gender: female = 1, male = 0
- user_age: days since registration at the last user login time
- paid_user: whether the user has ever used paid tier service
- downgraded: whether the user has ever downgraded

#### Activity measures
- artists: avg number of artists played per session
- songs: avg number of songs played per session
- length: avg length (playing songs only) per session
- interactions: avg user actions per session
- thumbs_down: avg thumbs down per session
- total_session: total sessions per user
- session_gap: avg days between each session

## Model Selection
Since we are predicting whether a user is at-risk to churn(1/0), the prediction is a **classification** problem. 4 classification algorithms are used for initial training:

- Logistic Regression
- Random Forest
- Linear SVC (Support Vector Classifier)
- Gradient-Boosted Tree Classifier

## Model Tuning
![Model Results]!(screenshots/results.JPG)

Two solutions are used to handle the imbalanced data:
- class weights
- changing threshold

## AWS EMR Deployment
The pipeline is deployed on AWS EMR with full data. 

Random forest performs better than logistic regression on the full dataset.

Please see Sparkify_spark_deploy.html for model result details.

