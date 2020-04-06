# yelp-recommendation-system
This project aims to practice how to deal with large data sets using PySpark. The main goal of the project is to use different techniques to build a recommend engine in order to recommend restaurants to Yelp users. 

## Business case
•	Goal: Build a recommendation engine to recommend restaurants to Yelp users using models
•	We used NLP to analyze reviews and categorize them as positive or negative
•	We use Association Rules based on user and category of food to recommend restaurants to users
•	We use ALS based on stars to recommend restaurants to users
•	We use Graph to recommend restaurants based on friends’ rating of restaurants and categories

## Data: 
•	8GB from Yelp and Jason format 
•	3 tables (business, reviews, user) 
•	Store in RCC (HDFS) first and due to limited resources we moved to it to Google Cloud
•	Google Cloud – We created a storage bucket to store the data and we create a cluster with 1 master node and 2 worker nodes 
•	Connected to JupyterNote for cleaning, exploratory analysis and modeling

## Cleaning & Exploratory analysis
•	We focus on the restaurants out of the all data and we only analyze data from the US
•	Data is from 2004 to 2018

•	Business itself: most restaurants are in LV and AZ is the state with largest number of restaurants. Most restaurants in the data are American food followed by Italian and Mexican

•	Reviews: McDonalds is the restaurant with largest number of 1star and the top 3 restaurants are Hash, Mon ami and n-out; people tend to rate more when they like the restaurants. 

•	User: people tend to give longer reviews for the restaurants they rate better (4 and 5 stars); Users: tend to rate more on Sundays

## Sentiment analysis
•	Goal: have a model to determine if reviews are positive or negative; We starts by changing everything to lower case, remove punctuation and stop words and tokenize sentences into words. 
•	We created a Pipeline to automatically do the cleaning steps
•	If the user gives starts => 4 stars – positive and then we used 2 ways to convert text into vectors: HashingTF and countVectorizer
### HashingTF (length features to 1000): convert all the words into vectors and then fit into a classification model:
    Logistic regression (best accuracy of positive or negative – 84% overall, the model it is not good capturing the negative reviews)
    Decision tree
    Gradient boost
### CountVectorizer:
    Logistic Regression (takes up more memory so we did not use other models) – 89% accuracy being better than HashingTF – Countervectorizer is able to capture negative reviews better but it takes more time to run the model and more memory required

## Modeling
•	Association Mining – we recommend the category of the restaurant users might go in the future according to the category restaurants that they went in the past. We try to predict the type of restaurants users are willing to try. If you like pizza (according to the restaurants you visited before, there is an higher chance to go to fast food places).

•	ALS – we recommend restaurants to users according to the city where they are based. In this case we used the top 3 cities with the largest number of restaurants (Las Vegas, Phoenix and Scottsdale). We use stars, restaurants and users and the algorithm uses stars to recommend the restaurants to users. We recommend restaurants to people and the other way around.

•	Graphs – we use the categories of restaurants that friends went to recommend restaurants to users are likely to go. We filter by categories (Mexican food) and starts (>4). In this case, we have one person with about 200 friends and all the restaurants they went, which are likely to suit the user. (grey = friend and red = restaurants) – More dense means more friends went so this restaurant may be a good fit for the user.

## Findings 
•	BUSINESS: Sentiment Analysis can help businesses to quicker classify as good or bad review without looking at the text; ALS can help restaurants targeting the users that are more likely to go to the restaurant; Graphs can help users to know what type of restaurants they will like according to their friend’s preferences 
•	USERS: Association mining can help businesses to give recommendations to users based on the preference of restaurants; ALS can give users the restaurants that best fit them  

## Limitations & Future work
•	Resources limitation
•	Better cleaning the data; Use geolocation to give more precise recommendations; Expand recommendation models to more cities and categories; Use pageRank to rank users according the weight of their reviews.

