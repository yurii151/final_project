
Link to our webpage/dashboard: https://yurii151.github.io/final_project/
Link to our Google Slides: https://docs.google.com/presentation/d/1AM1x0Q1oyPE18vvBQ19UnV1bJkfS3XROPo5NXjXs9BA/edit?usp=sharing

# Recommedation System:
Our goal with this project was to create a book recommendation system that will automatically suggest a book to a user based on their preferences.
We chose a recommendation system because popular platforms such as Netflix, Amazon, and Spotify use these systems to keep their users interested by recommending media or other items to them based on their current tastes, taking the guesswork out of having to choose their next show, item to purchase, or song. In our case, our end-user can use our recommendation system to help choose their next book.
### How Recommendation Systems Work
There are three common types of recommendation engines that are utilized in many industries: Content-Based Filtering, Collaborative Filtering, and Popularity Filtering. Content-Based filtering makes recommendations based on the content of the item For example, if one reader enjoyed an adventure novel, a content-based recommendation engine would recommend another adventure novel. In collaborative filtering, users are grouped together based on their similarities. For example, if a group of 20-30 year olds enjoyed the book, <i>Harry Potter and the Sorcerer's Stone</i>, and our user was in the 20-30 year old age range, the recommendation system would suggest the same book. Lastly, a popularity-based system uses a weighted rating to collect the average rating of the book and the count of ratings to create a list of most popular books.
### Our Recommendation System
For our project, we have chosen to do a popularity-based recommendation system and a content-based recommendation system using the data we collected from Kaggle.com
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

## Machine Learning:
    
    In the book_models.ipynb file in the Machine learing branch was where we did some exploration of our books.csv dataset that we compiled to see if there were any effective machine learning models that we could use in our book recommender. The first model that was tested on the dataset was the linear regression model. We would want the system to recommend a book if it has a rating above 3.5 Based on that condition, the linear regression model ran with 59.65% accuarcy with this classifcation report:

<img width="659" alt="Screen Shot 2022-03-27 at 3 52 55 PM" src="https://user-images.githubusercontent.com/92888170/160304600-7221aca7-5f40-4784-b31d-88f76f44ca40.png">

The next model tested was the random forest model. This model ran with 61.63% accuracy with this classification report:

<img width="414" alt="Screen Shot 2022-03-27 at 3 55 35 PM" src="https://user-images.githubusercontent.com/92888170/160304686-66927e0b-6d2b-4579-8501-3f6d8d304843.png">

The final model that was tested was the one probably most related to the book recommendation system. The third method we used was clustering. The first thing that we had to do was figure out how many clusters would be the most appropriate. 

<img width="647" alt="Screen Shot 2022-03-27 at 3 59 31 PM" src="https://user-images.githubusercontent.com/92888170/160304833-0f275aac-29c1-4a20-96e8-712fdbf210e4.png">

The elbow curve suggests that 2  clusters would be most optimal, so that is how many we test for the model.

<img width="647" alt="Screen Shot 2022-03-27 at 4 12 22 PM" src="https://user-images.githubusercontent.com/92888170/160305292-6c08d9b4-f562-44c4-b507-8aa93e6c0509.png">


### Pros and Cons:

By the way that our dataset was constructed, out of our first two models, the random forest model gives pretty good reccomendation. With precision of 0.95% and recall 0.63% for positive cases, this model does a fine job. However, a downside of this is that in terms of reccomending books, there is more than just averager rating, and that is the main component of the first two models. At first, we were content with using thes models. But we decided that the third model, clustering, is much more in the scope of the question we are trying to answer. Creating clusters is the best way to recommend similar books in our opinion, and clustering can achieve that. Going foward we will continue to investigate clustering and how to incorporate more in our project. We are currently solving the visual aspcet of the project but will continue to implent more elements going forward. 

