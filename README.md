# Module-24-Capstone-Assignment
This repo contains the files for the PCMLAI Module 24 Capstone Assignment

## Overview
In this Capstone Assignment, my goal is to compare the performance of various recommendation systems, namely Content-Based Filtering, Collaborative Filtering, and Hybrid Recommendation Systems. I will utilize datasets related to book ratings.  

## Getting Started
The datasets are from the Goodreads Book Grapgh Datasets. The ratings.csv dataset consists of ratings given to various books by Goodreads users. The books.csv dataset provides information about each book that is rated including the title, original publication year, ISBN, average rating, and more. These datasets can be found [here](https://cseweb.ucsd.edu/~jmcauley/datasets/goodreads.html#datasets) Goodreads has asked for the following citations to be provided upon use:
- Mengting Wan, Julian McAuley, "Item Recommendation on Monotonic Behavior Chains", in RecSys'18. [bibtex]
- Mengting Wan, Rishabh Misra, Ndapa Nakashole, Julian McAuley, "Fine-Grained Spoiler Detection from Large-Scale Review Corpora", in ACL'19. [bibtex]

It should be noted that the original datasets from the Goodreads website provided above were too large to load onto my computer. Therefore, I am using modified versions of the datasets from the GitHub user zygmuntz through their goodbooks-10k repository which can be found [here](https://github.com/zygmuntz/goodbooks-10k). The ratings.csv and books.csv datasets are what I will use. It should be noted that the modifications to the datasets are necessary for modeling. To be specific, the original ratings dataset consisted of Goodreads-specific user and book IDs. The modified ratings dataset gives a more uniform identification to the books and users. These identification numbers were provided by Goodreads with the purpose of being combined and added to the ratings dataset.

### Problem 1: Understanding the Data

To gain a better understanding of the data, please read the information provided in the Goodreads link above, and examine the **Overview** section of the page.

- All the data was collected in late 2017 from goodreads.com. Three groups of datasets were collected. These datasets include meta-data of the books, user-book interactions, and users' detailed book reviews. All three datasets can be joined on book, user, and review IDs. 

The ratings.csv dataset provides the ratings given to multiple different books from multiple different users. Each book has been reviewed by multiple users and each user has reviewed multiple books. In total, there are 3 columns (user_id, book_id, and rating) and 100,000 rows. The books.csv dataset provides information on each book that has been reviewed. 10,000 books have been reviewed. 

### Problem 2: Read in the Data

Use pandas to read in the datasets `ratings.csv` and `books.csv`and assign to a meaningful variable name.

### Problem 3: Understanding the Features

Examine the data description below, and determine if any of the features are missing values or need to be coerced to a different data type.

Input variables:
# Goodreads Book Graph Data:
1 - User-ID : Gives anonymity to each Goodreads user. With the User-ID one can search through the user_id_map.csv dataset to find the users' Goodreads user_id. (numeric)
2 - Book-ID : Allows each book to be identifiable while also keeping the data numeric and simple for modeling. With the Book-ID one can search through the book_id_map.csv dataset to find the books' Goodreads user_id. One can also search the books.csv for the specific title of the book and information about it. (numeric)
Output variable (desired target):
21 - Rating - How much does the member like the book? (numeric: 1-5 with 5 being the highest appreciation)

- There are no missing values or duplicate columns in the ratings.csv dataset. The books.csv dataset did have missing values but no duplicate values. The missing values/rows in the books.csv dataset have been dropped and the index has been reset.

### Problem 4: Perform Exploratory Data Analysis (EDA)

Examine the data and perform EDA to identify patterns in the data.

- During this time, some data analysis was performed to identify some features that are likely to affect the target outcome to help with modeling and predicting. The books that were reviewed are organized based on rating frequency. It should be noted that this does not include how well each book is rated. This only ranks the books based on the number of ratings they each received. The top fifty most frequently rated books are specifically listed in the Jupyter Notebook. The top five most frequently rated books include The Hunger Games, Harry Potter and the Sorcerer's Stone, Twilight, To Kill a Mockingbird, and The Great Gatsby respectively. The frequency of how much a book is rated affects the recommendation system. This is because trending books tend to be highly recommended and even receive high ratings due to their popularity.

- The users were organized based on how frequently they were listed in the data. The top 50 reviewers are listed. These users will contribute the most to the recommendation system as they review the most books. The mean of the top 50 most frequent users was calculated as 194.32. This means that on average, the top 50 most frequent users read and reviewed 194.32 books each. These users will affect the recommendation system the most as frequent users are altering the average rating and popularity of many books more often compared to users who read and review less often.

- The overall number of times each individual rating (1, 2, 3, 4, and 5) was given is visualized. It is shown that overall a rating of 4/5 was given the most with 5/5 being the second most given rating. Next is 3/5, 2/5, and 1/5 respectively. This is important to observe as most books that were read and reviewed received a 3/5 or above. Not many books received a low rating. This is likely because most of the rated books have been trending/popular at some point.

![Number of times each rating was given](https://github.com/user-attachments/assets/ebd652fa-c8ec-462e-affd-0b5b3449f7c2)

- The distribution of the average ratings for each book is visualized. This differs from the previous plot as the previous plot show how many times each rating was given while this plot shows how many times each average rating was given. The highest average falls around 4/5. Ratings between 4/5 and 4.5/5 seem to be the second highest while ratings between 3.5/5 and 4/5 are the third highest. Although showing a different column of data, this plot aligns with the visualization of the previous plot. Based on this distribution it can be inferred that this dataset lacks outliers as the average ratings and individual ratings show reasonable outcomes based on domain knowledge. To be specific, books that were trendy received high ratings. However, the dataset does show a bias for popular books and lacks diversity among the books to include unpopular yet highly rated books.

![Distribution of the average ratings](https://github.com/user-attachments/assets/8f3b6078-0ed6-4b3d-bb80-e39253a497a3)

- Within the 50 most frequently rated books, the 25 highest-rated books are found. The top five highest rated of the top 50 frequently rated books include Harry Potter #7, Harry Potter #6, Harry Potter #3, Harry Potter #4, and Harry Potter #5 respectively. This is interesting as none of these books were in the top 5 of the frequently rated books yet they were given the highest ratings. This will affect the recommendation system as a trending book with high reviews is likely to be recommended more than a trending book with okay reviews. 

![25 highest rated of the top most frequently rated books](https://github.com/user-attachments/assets/18dbb0d4-7eb6-4df1-8813-045eb98d5231)

### Problem 5: Understanding the Task

After examining the description and data, your goal now is to clearly state the *Business Objective* of the task.  State the objective below.

- Goodreads is a social cataloging website that allows people to find and share books. Goodreads users make and read reviews, search for books, create lists of books, create a library catalog, and get book recommendations. Considering Goodreads' purpose is to help its users find enjoyable books to read based on the users' own data and the data of similar users, a good recommendation system is important. The recommendation system ensures that the Goodreads site fulfills its purpose and gives each user unique book recommendations that they will actually enjoy. Goodreads must figure out what type of recommendation system is best for predicting the best recommendations for each user. Using recommendation systems to determine the best model, Goodreads will be able to create unique and accurate algorithms for each user. This would increase customer retention and encourage more people to join Goodreads as accurate recommendations will encourage book readers to use this website to find and share books.  

### Problem 6: Engineering Features

Now that you understand your business objective, we will build a basic model to get started.  Before we can do this, we must work to encode the data. Using just the ratings.csv information features, prepare the features and target column for modeling with appropriate encoding and transformations.

- The title and authors columns of the books dataset are merged into the ratings dataset. This is so that an item profile is built for each book. This will be especially useful for the content-based model. Then the authors and title columns are encoded with label encoding as the data must be numerical to utilize in the models.

### Problem 7: Train/Test Split

With your data prepared, split it into a train and test set.

- The data was split with the test size being 20%, leaving 80% of the data for training. The features that will be used for basic modeling are all the ones available in the rated_books dataset including the 'user_id', 'book_id', 'authors_encoded', and 'title_encoded' features. The rating column is the target column which lists how much the user likes the book on a scale of 1 to 5.

### Problem 8: A Baseline Model

Before we build our first model, we want to establish a baseline.  What is the baseline performance that our classifier should aim to beat?

- A population-based recommendation system model was used as the baseline. This model took about 0.0441 seconds to train. The precision and recall scores were calculated to evaluate the model. The precision score of the population-based recommendation system model is about 0.0038. The recall score of the population-based recommendation system model is about 0.0038. This is the baseline performance that we should aim to beat.

- It should be noted that precision and recall were used as evaluation metrics as they are good for recommendation systems. Both metrics evaluate the effectiveness of the recommendation system and the relevance of items. Precision focuses on the user and identifies the proportion of recommendations that are relevant to them. This ensures that users do not receive recommendations that they will dislike. Recall identifies the proportion of relevant items the user actually sees. This ensures that the user isn't missing out on recommendations that they would like. A balance of both precision and recall is preferred. However, if a balance cannot be acheived, a high precision, low recall is preferred as it ensures that the recommendations are relevant. High recall with low precision would mean that the user is receiving a lot of irrelevant recommendations.

### Problem 9: Content-Based Recommendation Systems
Compare multiple content-based models and find the best performing one to use for the hybrid recommendation system. 

- The first content-based recommendation system is a random forest classifier. This model took 10.0912 seconds to train. The precision score is 0.5769. The recall score is 0.6889. With an okay training time, this model gives more recommendations that aren't too relevant considering the recall is higher than the precision.

- The second content-based recommendation system is an ANNOY index. This model took 65.3006 seconds to train. The precision score is 0.0019. The recall score is 0.0093. With a significantly longer training time than the other content-based models and extremely low precision and recall scores, this model performed poorly. The precision score was even worse than the precision score for the baseline model.

- The third content-based recommendation system is an HNSW model. This model took 14.7510 seconds to train. The precision score is 0.5236. The recall score is 0.5244. Although this model took longer to train and has slightly lower precision and recall scores than the random forests recommendation system, the HNSW model's precision and recall scores are more balanced with each other. Therefore, this is the content-based model that will be used for the hybrid recommendation system.

### Problem 10: Collaborative-Based Recommendation Systems
Compare multiple collaborative-based models and find the best performing one to use for the hybrid recommendation system.

- Five collaborative-based recommendation systems are trained and evaluated together. It should be noted that these models were trained on a 5,000 row sample of the original training set as using the entire training set to train all five models at once was too computationally expensive. These models include KNNBasic, SVD, NMF, CoClustering, and SlopeOne. KNNBasic has a training time of 1.0304 seconds, a mean precision score of 0.7733, and a mean recall score of 0.2838. SVD has a training time of 0.5304 seconds, a mean precision score of 0.8197, and a mean recall score of 0.3033. NMF has a training time of 3.4157 seconds, a mean precision score of 0.7954, and a mean recall score of 0.2928. CoClustering has a training time of 3.4655 seconds, a mean precision score of 0.8100, and a mean recall score of 0.2988. SlopeOne has a training time of 0.0000 seconds, a mean precision score of 0.8185, and a mean recall score of 0.3029. SVD was considered the optimal algorithm. However, SlopeOne has the next best mean precision and mean recall scores. SlopeOne also has no parameters to tune and has a short training time. In comparison to SVD, SlopeOne is also easier to implement and less computationally expensive. Therefore, the SlopeOne model will be used for the hybrid recommendation system.

- It should be noted that the SlopeOne model was redone on its own to provide ease when creating and comparing the hybrid recommendation systems. It was also redone on the entire training set rather than just a sample. When it comes to the SlopeOne model on the entire training set, the only change is in its training time which increased to 61.5599 seconds. The precision and recall scores stayed the same.

### Problem 11: Hybrid Recommendation Systems
Compare multiple hybrid models (content and collaborative-based recommendation systems) and find the best performing one for Goodreads to use. I will use the HNSW content-based model and the SlopeOne collaborative-based model.

- The first hybrid recommendation system is a weighted recommendation system. This model took 0.0002 seconds to train. The precision score is 0.3663. The recall score is 0.5350.

- The second hybrid recommendation system is a switching recommendation system. This model took 9.1791 seconds to train. The precision score is 0.6300. The recall score is 0.6300.

- The third hybrid recommendation system is a mixed recommendation system. This model took 8.4877 seconds to train. The precision score is 0.5527. The recall score is 0.5549. 

- The fourth hybrid recommendation system is a cascade recommendation system. This model took 8.6091 seconds to train. The precision score is 0.5454. The recall score is 0.5478.

- The fifth hybrid recommendation system is a feature recommendation system. This model took 5.9364 seconds to train. The precision score is 0.0004. The recall score is 0.0020. The extremely low precision and recall scores are due to a lack of diversity in the HNSW content-based recommendations. To increase diversity the hyperparameters ef_construction and M can be increased. However, the more they are increased, the more computationally expensive the model becomes.

- Training times come from a full run of the entire notebook. When I went back and reran the training cells the training times decreased significantly. However, for the sake of uniformity, the original training times are listed.

### Findings

- As a reminder, the goal of creating these models is to help Goodreads establish a recommendation system that can accurately predict books that each user will enjoy. This will help Goodreads retain their current customers and gain more customers as they will find the recommendations they receive to be accurate to their interests. Goodreads will make more money and users will have an enjoyable experience reading and finding books. After some exploratory data analysis, it is shown that most of the books that were reviewed had high average scores. This is important as Goodreads shows and considers the average score when making recommendations, especially collaborative-based ones. However, individual ratings are also important as users may not always like popular books, which is where content-based recommendations come in. Overall, the hybrid recommendation systems yielded the best outcomes in terms of training times, precision scores, and recall scores.
  
- Of the five hybrid recommendation systems that were created and compared, the switching hybrid model seems to have performed the best. Although this model's training time is the highest training time of the hybrid recommendation systems, it only took a little over 9 seconds. The main contributions to this model being deemed the best are its precision and recall scores. This model has the highest and most balanced scores. The precision and recall scores are also identical meaning the users are receiving a higher proportion of recommendations, and of those recommendations, a higher proportion of them are relevant compared to the other hybrid recommendation systems.
  
- Considering the users in this dataset seem to have given a high amount of reviews (gives more data) and the books that are reviewed are mostly popular ones, a switching hybrid recommendation system is great as it switches from the content-based model for new users to avoid only recommending irrelevant yet popular books to the collaborative model as new data is added for the model to see where in the collective group does the user fall.

### Next Steps

- Further data collection, data cleaning predictive modeling, and hyperparameter tuning are best. Data collection would be the most important. This is because most of the users in the dataset have reviewed many books. The average number of books read/reviewed given by the top reviewers was 194.32 books/reviews. However, new users with little data cannot rely as much on content-based filtering as much as older users. Data with books that are not only trendy is also important. Not everyone enjoys reading trendy books all the time. Some people want new and niche book recommendations. The current dataset has a bias toward popular books. It would be beneficial to include untrendy books in the dataset to help strengthen the content-based aspect of the recommendation system so that people aren't receiving the same book recommendations over and over without uniqueness in their own algorithm. With new data will also need data cleaning. However, it should be mentioned again that the dataset used in the project was already put together and did not come directly from the Goodreads website as the original dataset was too big to download and open on my computer. Therefore, a dataset that isn't so computationally expensive would also be helpful. There are many types of hybrid recommendation systems, therefore, exploring as many as possible would be helpful. The most important next step is hyperparameter tuning. It should be noted that the data sizes and parameters used in these models were chosen to optimize the models to save time. This does not necessarily mean that the switching model is the only model that will yield success through high and balanced precision and recall scores. However, the Goodreads dataset is large which caused the models to be computationally expensive. Recreating these models with smaller groups of data, like choosing one genre of books, may be more beneficial and less computationally expensive. Another option is to choose other content-based and collaborative-based recommendation systems to use in the hybrid recommendation system. The next collaborative-based model that should be used in the hybrid recommendation system is SVD as this was determined to be the optimal collaborative-based model.

The link to the Jupyter Notebook with the coding for this project can be found [here](https://github.com/nadiaspn0503/Module-24-Capstone-Assignment/blob/main/Module%2024%20Capstone.ipynb).
