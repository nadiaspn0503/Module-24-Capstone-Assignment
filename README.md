# Module-24-Capstone-Assignment
This repo contains the files for the PCMLAI Module 24 Capstone Assignment

## Overview
In this Capstone Assignment, my goal is to compare the performance of various recommendation systems, namely Content-Based Filtering, Collaborative Filtering, and Hybrid Recommendation Systems. I will utilize a datasets related to book ratings.  

## Getting Started
The datasets are from the Goodreads Book Grapgh Datasets. The ratings.csv dataset consists of ratings given to various books by goodreads' users. The books.csv datset provides information about each book that is rated including the title, original publication year, ISBN, average rating, and more. These datset can be found [here](https://cseweb.ucsd.edu/~jmcauley/datasets/goodreads.html#datasets) Goodreads has asked for the following citations to be provided apon use:
- Mengting Wan, Julian McAuley, "Item Recommendation on Monotonic Behavior Chains", in RecSys'18. [bibtex]
- Mengting Wan, Rishabh Misra, Ndapa Nakashole, Julian McAuley, "Fine-Grained Spoiler Detection from Large-Scale Review Corpora", in ACL'19. [bibtex]

It should be noted that the original datasets from the goodreads website provided above were too large to load onto my computer. Therefore, I am using motified versions of the datasets from the GitHub user zygmuntz through their goodbooks-10k repository which can be found [here](https://github.com/zygmuntz/goodbooks-10k). The the ratings.csv and books.csv datasets are what I will use. It should be noted that the motifications to the datasets are necessary for modeling. To be specific, the original ratings dataset consisted of Goodreads specific user and book ids. The modified ratings dataset gives a more uniform identification to the books and users. These identification numbers were provided by Goodreads with the purpose of being compined and added to the ratings dataset.

### Problem 1: Understanding the Data

To gain a better understanding of the data, please read the information provided in the Goodreads link above, and examine the **Overview** section of the page.

- All the data was collected in late 2017 from goodreads.com. There were three groups of datasets that were collected. these datasets include meta-data of the books, user-book interactions, and users' detailed book reviews. All three dataset can be joined on book, user, and review ids. 

The ratings dataset provides the ratings given to multiple different books from multiple different users. Each book has been reviewed by multiple users and each user has reviewed multiple books. In total, there are 3 columns (user_id, book_id, and rating) and 100,000 rows. The books dataset provides information on each book that has been reviewed. There are 10,000 books that have been reviewed. 

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

- During this time, some data analysis was reformed to identify some features that are likely to affect the target outcome to help with modeling and predicting. The books that were reviewed are organized based on rating frequency. It should be noted that this does not include how well each book is rated. This only ranks the books based on the number of ratings they each recieved. The tope fifty most frequently rated books are specifically listed in the jupyter notebook. The top five most frequently rated books include The Hunger Games, Harry Potter and the Sorcerer's Stone, Twilight, To Kill a Mockingbird, and the Great Gatspy respectively. The frequency of how much a book is rated affects the recommendation system. This is because trending books tend to be highly recommended even recieve high ratings due to popularity.

- The users were organized based on how frequently they are listed in the data. The top 50 reviwers are listed. These users will contribute the most to recommendation system as they review the most books. The mean of the top 50 most frequent users was calculated as 194.32. On average, the top 50 most frequent users read and reviewed 194.32 boks each. These users will affect the recommendation system the most as frequent users are altering the average rating and popularity of many books more often compared to users that read and review less often.

![Number of accepted subscriptions by job](https://github.com/user-attachments/assets/73aa7e9f-159c-436d-8704-6d56e18b2c8f)

- During data analysis, the jobs admin, technician, student, and retired were chosen for comparison. Despite the fewer subscriptions, fewer retired people and students were contacted, making the percentage who subscribed within those specific groups high. The comparison was made between married individuals with a technician, admin, student, or retired job to all others. The results showed that of the married individuals with a technician, admin, student, or retired job, 14.47% said "yes" to subscribing while 10.83% of all others said "yes". This shows that these jobs and a married status are characteristics that are likely to lead a person to say "yes" to a subscription. It should be noted that the high "no" rate has been observed but will not be extremely important considering how significantly unbalanced the data is.

![Subscription Acceptance for Married People With a Technician, Admin, Student, or Retired  Job to All Others](https://github.com/user-attachments/assets/1106147f-2f40-445d-9f5b-078bc73ef8db)

- Another comparison performed was between those with a university degree and no personal loan and all others. These features were chosen as those with a university degree are likely to make more money than those without one. The lack of personal loans means that these individuals likely do not have any major debt, like a student loan, that would prevent them from saying "yes" to a subscription. The results of the analysis show that 13.98% of those with a university degree and no personal loan said "yes" to a subscription while 10.40% of all others said "yes".

![Subscription Acceptance for People With a University Degree and No Personal Loan to All Others](https://github.com/user-attachments/assets/bd4fd47e-b809-4baa-8cfb-7dec9e97a348)

### Problem 5: Understanding the Task

After examining the description and data, your goal now is to clearly state the *Business Objective* of the task.  State the objective below.

- Goodreads is a social cataloging website that allows people to find and share books. Goodreads users make and reads reviews, search for books, create lists of books, create a library catalog, and get book recommendations. Considering Goodreads' purpose is to help its users find enjpyable books to reads based on the users' own data and the data of similar users, a good recommendation system is important. The recommendation system ensures that the Goodreads' site fulfills its purpose and gives each user unique book recommendations that thwy will actually enjoy. Goodreads must figure out what type of recommendation system is best for predicting the best recommendation for each user. Using recommendation systems to determine the best model, Goodreads will be able to create unique and accurate algorithms for each user. This would also encourage more people to join Goodreads as accurate recommendations will encourage book readers to use this website to find and share books.  

### Problem 6: Engineering Features

Now that you understand your business objective, we will build a basic model to get started.  Before we can do this, we must work to encode the data.  Using just the ratings information features, prepare the features and target column for modeling with appropriate encoding and transformations.

- Considering all columns of the ratings.csv dataset (the dataset that will be used for modeling) are integers, no encoding is required. Although not all columns in the books.csv dataset are integers, this dataset is only meant for EDA not modeling. Therefore, the books.csv dataset does not require encoding.

### Problem 7: Train/Test Split

With your data prepared, split it into a train and test set.

- The data was split with the test size being 20%, leaving 80% of the data for training. The features that will be used for basic modeling are all the ones available in the ratings.csv dataset including the 'user_id' and 'book_id' features. The rating column is the target column which lists how much the user likes the book on a scale of 1 to 5.

### Problem 8: A Baseline Model

Before we build our first model, we want to establish a baseline.  What is the baseline performance that our classifier should aim to beat?

- A population-based recommendation system model was used as the baseline. This model took about 0.0323 seconds to train. The mean squared error (mse) was caluclated to evaluate the model. The mse score of the population-based recommendation system model is about 1.1101. This is the baseline performance that we should aim to beat.

### Findings

- As a reminder, the goal of creating these models is to help the Portuguese banking institution predict if a customer will subscribe to the term deposit or not. This will help the company save time when it comes to its marketing campaigns as these campaigns involve individually calling customers multiple times and talking to them for various and long durations. Being able to know what features and characteristics subscribing customers are likely to have will help them target the campaigns to individuals who will likely subscribe. After some data analysis and predictive modeling, it seems that a decision tree model is the best for predicting whether a customer will subscribe or not. The various factors that went into this result include the times, recall scores, and confusion matrices of the models. It should be noted that the main two models that performed well were the logistic regression and decision tree models. The knn and svm models were very computationally expensive, especially after hyperparameter tuning. Although the knn model had a slightly higher recall score, the difference is not significant and the confusion matrix for this model shows many more false positives compared to other models. The knn model with n=10 showed a better confusion matrix than the knn model with n=1; however, this confusion matrix was no better than the other models. In addition to being computationally expensive, the svm model did not show better results when a polynomial kernel was added. Between a logistic regression model and a decision tree model, the decision tree model was less computationally expensive both with and without a specified maximum depth. Furthermore, decision trees work best for this dataset as they are better for imbalanced data and are not biased toward the majority class.

### Next Steps

- Further data collection, data cleaning, and predictive modeling are best. Other features that may affect a subscription decision should be considered like the number of children someone may have or the individual's income. Income is especially important as the 'job' feature seems to be effective during prediction. However, some jobs may seem like they usually yield a high income when they actually don't. For example, someone who is self-employed or an entrepreneur could make a wide range of money that even varies throughout the year significantly. Further data cleaning could be done to better encode the non-numerical data. It should be noted that to know exactly what label was encoded to which feature, label encoding was done individually. Using the LabelEncoder() function would be faster in the future. Further feature selection could also be done. For the models created, features specific to the individuals who were called were used, like 'job' and 'marital' status. However, other features like the unemployment rate and month may be useful in predicting the best time to contact customers. Further parameter tuning would also be useful. Considering the duration of the call was important yet was not useful for prediction, the Portuguese banking institution could also try other marketing campaigns like in-person ones that help lessen the duration and contact while increasing the reach. Personally talking to someone can help when trying to convince them of something in comparison to talking on the phone.

The link to the Jupyter Notebook with the coding for this project can be found [here](https://github.com/nadiaspn0503/Module-17-Practical-Assignment-3/blob/main/prompt_III.ipynb).
