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

Import the data here 

### Assessment of Missingness


### Hypothesis Testing

### Framing a Prediction Problem

### Baseline Model

### Final Model

### Fairness Analysis
