
<p>Link to our webpage/dashboard: https://yurii151.github.io/final_project/</p>
<p>Link to our Google Slides: https://docs.google.com/presentation/d/1AM1x0Q1oyPE18vvBQ19UnV1bJkfS3XROPo5NXjXs9BA/edit?usp=sharing</p>

# Recommedation System:
Our goal with this project was to create a book recommendation system that will automatically suggest a book to a user based on their preferences.
We chose a recommendation system because popular platforms such as Netflix, Amazon, and Spotify use these systems to keep their users interested by recommending media or other items to them based on their current tastes, taking the guesswork out of having to choose their next show, item to purchase, or song. In our case, our end-user can use our recommendation system to help choose their next book.
### How Recommendation Systems Work
There are three common types of recommendation engines that are utilized in many industries: Content-Based Filtering, Collaborative Filtering, and Popularity Filtering. Content-Based filtering makes recommendations based on the content of the item For example, if one reader enjoyed an adventure novel, a content-based recommendation engine would recommend another adventure novel. In collaborative filtering, users are grouped together based on their similarities. For example, if a group of 20-30 year olds enjoyed the book, <i>Harry Potter and the Sorcerer's Stone</i>, and our user was in the 20-30 year old age range, the recommendation system would suggest the same book. Lastly, a popularity-based system uses a weighted rating to collect the average rating of the book and the count of ratings to create a list of most popular books.
### Our Recommendation System
For our project, we have chosen to do a popularity-based recommendation system and a content-based recommendation system using the data we collected from Kaggle.com

### Recommendation System Activity Diagram
<img width="1600" alt="Activity Diagram"
src="https://github.com/yurii151/final_project/blob/308448ecdb85f8466d8bed3b8b1b138f7be15556/Images/activitydiagram.jpg">
     
### Our thoughts
There are so many different inputs that the model can look at with a recommendation system, It seemed like a broad and relevant topic in today’s Data Science realm and a good opportunity to practice a number of the skills that we learned over the course of the past several month. As an added bonus, we all enjoy reading! 

## Resources and Data Exploration:

Data Source: [Goodreads Books.csv sourced by Nilim-Kaggle](https://www.kaggle.com/code/snanilim/book-recommendation-engine/data)
Software: Python 3.7.6,  sqlite, Jupyter Notebook

Our Dataset was sourced from Kaggle. It was a large dataset and included most of the categories which we wanted to explore. We started by attempting to merge a few of the datasets that we found to see if we could combine them to get a larger dataset with more categories to explore. This was unsuccessful as there were not enough overlap in books. 
One of the main categories that we was missing was the books genre. This was one of the main ways that we were hoping to group our books for recommendation so we wrote a scraping code to search for the book title on google and based off a list of genres scrape the genre for each of the books on our list. 
The code for scraping the books genre ran successfully, unfortunately we ran into issues with HTTP Error 429: Too Many Requests and in the end moved forward with dummy data. 
We chose SQLITE for our database as it suited the size of our dataset and our need for this project. 

# Visualizations:

Question we want answered: The purpose of this project is to try and use users data as well as book information to predict a book that the user would like. So the question that we want answered is what are the key features in figuring out what book will be recommended based on the data set. 

In addition to the following [google slides](https://docs.google.com/presentation/d/1AM1x0Q1oyPE18vvBQ19UnV1bJkfS3XROPo5NXjXs9BA/edit#slide=id.p) we will be creating a webpage hosted through GitHub Pages. 

## Tableau Visuals:

<img width="600" alt="Screen Shot 2022-04-05 at 4.59.24 PM"
 src="https://github.com/yurii151/final_project/blob/022bd190262a809ee4de72124644467342607747/Screen%20Shot%202022-04-05%20at%204.59.24%20PM.png">
 
 This bar graphs tells shows us the most popular genres based on book ratings. 
  1) Non Fiction
  2) Thriller
  3) Adventure 
  4) Romance 
  5) Fiction
 
 <img width="550" alt="Screen Shot 2022-04-04 at 6.44.37 PM.png" src="https://github.com/yurii151/final_project/blob/022bd190262a809ee4de72124644467342607747/Screen%20Shot%202022-04-04%20at%206.44.37%20PM.png">
      
Looking at user demographics this shows us that majoirty of our book lovers are between the ages 25 to 35. 
      
<img width="550" alt="User Location.png" src="https://github.com/yurii151/final_project/blob/022bd190262a809ee4de72124644467342607747/User%20Location.png">

Density map shows that our readers are located in the US and a high volume of them are located in the east coast.

## Machine Learning Overview:
### Data Preprocessing
Because we sourced the data from a cleaned dataset source from Kaggle, there was very minimal data preprocessing that had to be done. 
### Description of feature engineering and the feature selection
In our dataset, we wanted to see if we are able to make any type of prediction with the data we had at hand. We wanted to answer the question, <b>"Can our data suggest a book with a simple <i>yes or no</i> using the average rating field?"</b> In order to test this, we choose to create a new feature called = 'recommend'
```
# Convert the target column values to 1 and 0 based on their values
# if average rating is less than 3.5 = 0
# if average rating is more than 3.5 = 1
df['recommend'] = np.where((df['average_rating'] >= 3.50), 1, 0) 

# Drop the null columns where all values are null
df = df.dropna(axis='columns', how='all')

# Drop the null rows
df = df.dropna()

df.reset_index(inplace=True, drop=True)

df.head(50)
```
After creating the column we are going to base our prediction on, we needed to set up our features and drop the columns we didn't need to begin our testing process.
```
# Create our features
X = pd.get_dummies(df, columns=['bookID', "average_rating", "language_code", "text_reviews_count",
                                "ratings_count", "recommend"])
                                
#print(pd.get_dummies(X))

X = df.drop(columns=['recommend','title','authors','isbn','isbn13','publication_date','publisher', 'language_code'])
y = df["recommend"]
```

### Training and Testing
We are then left with the following counts: 
```
y.value_counts()
1    10390 (yes recommend)
0      733 (no recommend)
Name: recommend, dtype: int64
```
The following code is how we split our data for modeling:
```
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=1)
Counter(y_train)
Counter({0: 567, 1: 7775})
```
## Machine Learning Models:
For our modeling step, we chose 3 models to test: Logistic Regression (supervised), RandomForestClassifier (supervised), and KMeans Clustering (unsupervised). We used the 2 supervised learning models to test because we had a predictive 'recommend' column and included the clustering model to uncover any relationships between the book titles and other features.
#### LogisticRegression with OverSampling
We chose oversampling for this test because we had a low 'no recommend' value count at 733 (difference of 9657). Random Oversampling removes the bias from the minority class and adds to the training data set.
<b>This model provided a 59.65% accuarcy score</b> with this classifcation report:

<img width="659" alt="Screen Shot 2022-03-27 at 3 52 55 PM" src="https://user-images.githubusercontent.com/92888170/160304600-7221aca7-5f40-4784-b31d-88f76f44ca40.png">

#### RandomForestClassifier 
We choose to use Random Forest Classifier because we realize that the answer to our question would be a categorical value (yes or no) versus a numerical value. 
<b>This model ran with 61.63% accuracy score</b> with this classification report:

<img width="414" alt="Screen Shot 2022-03-27 at 3 55 35 PM" src="https://user-images.githubusercontent.com/92888170/160304686-66927e0b-6d2b-4579-8501-3f6d8d304843.png">

#### KMeans Clustering
The final method we wanted to test with our data with was an unsupervised model, KMeans Clustering. Unlike our previous 2 models, we knew what we wanted our model to predict for us (yes recommend or no recommend), we chose to include an unsupervised model to uncover any relationships between the data that we cannot explicitly see. 

<img width="647" alt="Screen Shot 2022-03-27 at 3 59 31 PM" src="https://user-images.githubusercontent.com/92888170/160304833-0f275aac-29c1-4a20-96e8-712fdbf210e4.png">

The elbow curve suggests that 2  clusters would be most optimal, so that is how many we test for the model.

<img width="647" alt="Screen Shot 2022-03-27 at 4 12 22 PM" src="https://user-images.githubusercontent.com/92888170/160305292-6c08d9b4-f562-44c4-b507-8aa93e6c0509.png">

### ML Results
In our two supervised learning models, we had accuracy scores between 59% - 61%. We cannot say these two models provide a high accuracy score in order to predict whether or not to recommend a book. It is also important to look at our models' F1 scores. Both supervised models provided an accuracy score between 75% - 76%. Though still not enough to confidently make predictions, it is still important to note that the <i>harmonic mean</i> is above 50%, meaning that there is a balance between the precision and sensitivity scores. Our unsupervised model allowed us to find the optimal clustering relationship and which book titles to group together.

## Finally, building our recommendation system
Now that we have explored the data with the help of Machine Learning models, we wanted to create a recommendation system that <i>actually</i> recommends books to a user!

#### Simple Recommendation System
Using a weighted average rating, we were able to populate the top rated book titles based on their average rating and ratings count
<img width="647" alt="weighted_rating" src="https://github.com/yurii151/final_project/blob/ea0a234f70896f265f34007bd2c86c39e41add7c/Images/Screen%20Shot%202022-04-09%20at%209.36.00%20AM.png">

#### Content-Based Recommendation System
Using Natural Language Processing Methods, we were able to create a similarity score and group similar book titles together based on similar features.

<img width="647" alt="content-based" src="https://github.com/yurii151/final_project/blob/ea0a234f70896f265f34007bd2c86c39e41add7c/Images/Screen%20Shot%202022-04-09%20at%209.36.56%20AM.png">

#### Recommendation Demo
<img width="647" alt="demo" src="https://media.giphy.com/media/qXWV8NruKKsPP1dv4i/giphy.gif">

