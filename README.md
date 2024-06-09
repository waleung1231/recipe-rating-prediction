### Introduction
Our dataset is focused on recipes. Based on this data, we want to answer the question: What factors determine if a recipe is given a high rating? We want to answer this question so that when we search online recipe sites we can look at small details and determine if a recipe is good to follow. 

The recipes dataset contains 234429 rows and the columns 'name', 'id', 'minutes', 'contributor_id', 'submitted', 'tags', 'nutrition', 'n_steps', 'steps', 'description', 'ingredients', 'n_ingredients', 'user_id', 'date', 'rating', and 'review'. 
| Column Name| Description |
|----------- | ----------- |
| name | the name of the recipe |
| id | the id of the recipe |
| minutes | how many minutes the recipe takes |
| contributor_id | the id of the person who submitted the recipe|
| tags | the hashtags the recipe was given |
| nutrition | a list of nuitrition data (calories, total fat, sugar, sodium, protien, saturated fat, carbohydrate) |
| n_steps | the number of steps theis recipe has |
| steps | text description of each step |
| description | a paragraph description of the recipe |
| ingredients | a list of ingredients |
| n_ingredients | the number of ingrdients |
| user_id | the id of the user that submitted the review  |
| date | the date the review was posted |
| rating | the rating the user gave the recipe (0-5) |
| review | the review that was given to the recipe by the user |


### Data Cleaning and Exploratory Data Analysis
We started off with spliting all the values in nuitrition into new seperate columns (calories, total fat, sugar, sodium, protien, saturated fat, carbohydrate). By splitting them into seperate columns we were able to see if different segmants of nuitrition may have affects to the ratings. E.g. if higher calories food can lead to higher ratings. 

In the 'rating' column, we changed all values with rating of 0 to np.NaN. This is because if a review contained no rating, it was a missing rating, and thus further analysis must be done to determine why it is missing.

After this step, we created an 'average_rating' column, where each recipe has an average_rating based on the mean of all ratings given by reviewers. 

For our univariate analysis, we decided to look at the distributin of the 'ratings' column. This is especially relevant because our model is predicting rating so we want to see what the distribution is like before we start training. The plot is heavily skewed left, towards 5-star ratings. 
[Insert plot here]

For our bivariate analysis we wanted to examine the relationship between number of ingredients ('n_ingredients') and number of steps ('n_steps'). 
[Insert plot here]

As you can see above, there does not appear to be a relationship, as there is a large variation in number of steps in recipes with similar number of ingredients. 

For our aggregation, we wanted to see the distribution of 'calories (#)', 'n_ingredients', and 'n_steps', so we created the pivot table below with the aggregation method 'mean'. 
[Insert table here]

While mean is not the most illustrative measure of center, in the pivot table on average recipes with higher ratings have slightly more ingredients and slightly lower number of steps. That being said, because the data is relatively similar, it is unlikely that rating is influenced solely by these factors. More exploration is needed. 

### Assessment of Missingness
We have 4 columns with relevant proportions of missing data: 'description', 'review', 'rating', and 'average_rating'. Of these 4 columns, we believe that 'description' and 'review' are NMAR because users chose not to submit them because they didn't think they were relevant. 

Meanwhile, we believe that 'rating' or 'average_rating' is MAR. Below is the plot of our permutation test to see if 'average_rating' is MAR conditioned on 'n_steps'.
[Insert n_steps plot here]

For this plot, our p-value was 0.0, meaning the results are statistically significant. This also applies for our permutation test to see if 'average_rating' is MAR conditioned on 'n_ingredients'. 
[Insert n_ingredients plot here].

Finally, we ran a permutation test to see if 'average_rating' was MAR conditioned on another numerical column, 'sodium (PDV)'. In the permutation test below you can see that our results are not statistically significant. 
[Insert sodium plot here].

Since 'average_rating' is MAR conditioned on 'n_steps' and 'n_ingredients', and 'average_rating' is based on 'rating', there is a high chance 'rating' is heavily related to 'n_steps' and 'n_ingredients'. We will use this information in our model. 

### Hypothesis Testing
For our hypothesis test, we wanted to answer the question: Do recipes with a higher number of ingredients have higher average ratings? 

We checked if recipes with a higher number of ingredients have the same average_ratings as recipes with a lower number of ingredients.

**Null hypothesis**: The distribution of average ratings for recipes with a high number of ingredients is the same as the distribution of average ratings for recipes with a low number of ingredients.
**Alternate hypothesis**: The distribution of average ratings for recipes with a high number of ingredients is different from the distribution of average ratings for recipes with a low number of ingredients.

For our test statistic we used the K-S statistic with a p-value cutoff of 0.05. 

Our test gave us a K-S statistic of 0.181 and a p-value of 1.47e-16. Based on these results, we reject our null hypothesis that the distribution of ratings for recipes with a high number of ingredients is the same as the distribution of ratings for recipes with a lower number of ingredients. This means 'n_ingredients' is significant for determining a recipe's rating, a fact we will use in our model. 

### Framing a Prediction Problem
For our prediction problem, we want to predict the rating of a recipe using a classification model. We have established that ratings (and thus average ratings) are heavily correlated with the numerical columns, such as n_ingredients and n_steps, meaning a relationship can be determined. Our classifier will be a multiclass classification model taking in these correlated rows such as n_ingredients and n_steps to predict whether a recipe is 1, 2, 3, 4, or 5 stars. We will evaluate the effectiveness of our model using accuracy because it tells us whether or not the model was successful at predicting the correct category. We chose accuracy over the F-1 score because it is easier to interpet in the context of our question - that is, if a recipe has been classified as having a correct rating. 

At the time of classification, we can assume the n_ingredients and n_steps would be known, so it is okay to use these as variables. 

### Baseline Model
For our baseline model we will use a DecisionTreeClassifier to classify our recipes data as having a rating of 1, 2, 3, 4, or 5 stars. In this baseline model, we are using the 2 quantative columns, n_steps and n_ingreidents, to predict whether what rating a recipe will receive. To prevent outliers from skewing the data, we decided to standardize these columns with StandardScaler(). 

Our train/test split was the default 75%/25%. On training data across all 5 categories our model had an mean accuracy of ~77.4%. On testing data, across all 5 categories it had a mean accuracy of ~77.1%. 

We also tried a RandomForestClassifier, but the results were roughly the same as the DecisionTreeClassifier, so we used the DecisionTreeClassifier as our final baseline model. Because the training/testing accuracy scores of our DecisionTreeClassifier model are very similar, we believe we didn't overfit the data. Therefore, the high accuracy means our baseline model is good. That being said, we believe it can be improved by testing categorical columns and seeing if they improve our model's accuracy. 

### Final Model

### Fairness Analysis
