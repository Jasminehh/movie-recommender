



## Basic Recommendation System

Our basic recommendation system can tell you what movies are most similar to your movie choice.

- Number of Users: 671
- Number of Movies: 9064

First we create a userId-movieTitle matrix. Each cell will be the rating that the user gave to that movie. Note there will be a lot of NaN values, because most people have not seen most of the movies.

### Most rated movies

![](image/mostRates.png)

### What Movies Most Correlated to "Forrest Gump"
We sort the dataframe by correlation, we should get the most similar movies, however note that we get some results that don't really make sense.
This is because there are a lot of movies only watched once by users who also watched forrest gump (one of the most popular movie).

![](image/fg_1.png)

Then we fix this by filtering out movies that have less than 100 reviews.

![](image/fg_2.png)

### What Movies Most Correlated to "The Silence of the Lambs"

![](image/silence.png)

## Advanced Recommendation Systems

Two most common types of recommendation systems are Content-Based and Collaborative Filtering (CF).

### Collaborative Filtering

- Memory-based models are based on similarity between items or users, where we use cosine-similarity.
- Model-based model is based on matrix factorization where we use SVD to factorize the matrix.

#### Model 1: Memory-Based Collaborative Filtering - Cosine Similarity
- User-Item Collaborative Filtering: “Users who are similar to you also liked …”
- Item-Item Collaborative Filtering: “Users who liked this item also liked …”

A distance metric commonly used in recommendation systems is cosine similarity, where the ratings are seen as vectors in n-dimensional space and the similarity is calculated based on the angle between these vectors.


#### Step 1: Create the user-item matrices for both testing and training data.
#### Step 2: Use the pairwise_distances function from sklearn to calculate the cosine similarity.
Cosine similarity for users k and a can be calculated using the formula below, where you take dot product of the user vector uk and the user vector ua and divide it by multiplication of the Euclidean lengths of the vectors.
![](image/cos.png)

#### Step 3: Make predictions by applying the following formulas:
##### User-based CF
![](image/user_based_cf.png)

We can look at the similarity between users k and a as weights that are multiplied by the ratings of a similar user a (corrected for the average rating of that user). You will need to normalize it so that the ratings stay between 1 and 5 and, as a final step, sum the average ratings for the user that you are trying to predict.

##### Item-based CF
![](image/item_based_cf.png)

When making a prediction for item-based CF we don't need to correct for users average rating since query user itself is used to do predictions.

#### Step 4: Use RMSE to evaluate accuracy of the predicted ratings.

![](image/rmse_1.png)

Memory-based algorithms are easy to implement and produce reasonable prediction quality. The drawback of memory-based CF is that it doesn't scale to real-world scenarios and doesn't address the well-known cold-start problem, that is when new user or new item enters the system.

#### Model 2: Model-Based Collaborative Filtering - SVD

Model-based Collaborative Filtering is based on matrix factorization which has received greater exposure, mainly as an unsupervised learning method for latent variable decomposition and dimensionality reduction. Matrix factorization is widely used for recommender systems where it can deal better with scalability and sparsity than Memory-based CF.

##### SVD
Collaborative Filtering can be formulated by approximating a matrix X by using singular value decomposition.
The general equation can be expressed as follows:

![](image/svd.png)

Elements on the diagonal in S are known as singular values of X.

Matrix X can be factorized to U, S and V. The U matrix represents the feature vectors corresponding to the users in the hidden feature space and the V matrix represents the feature vectors corresponding to the items in the hidden feature space.

User-based CF RMSE: 3.09
- SVD can be very slow and computationally expensive.
- Apply stochastic gradient descent to reduce RMSE.
- Use regularization terms to prevent overfitting.
