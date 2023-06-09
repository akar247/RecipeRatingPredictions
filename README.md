### <u> Overview</u>: 

Previously we completed an EDA exploratory data analysis on a recipe rating dataset from [food.com](https://dsc80.com/project3/recipes-and-ratings/food.com). This analysis can be found [here](https://akar247.github.io/RecipesDurationAnalysis/). Through this website we have documented our process for creating predictions on the dataset. 

<br>

---


<br>


## <strong> Framing the Problem </strong> 
<br>

With this dataset that contains information such as the recipe name, recipe cooking time, certain tags associated with the recipe, steps, ingredients, etc, we hope to utilize certain features to create a regression model on the average rating on a recipe. 

<br>

Specifically, the response variable we are predicting is the average rating associate with each recipe given certain features. The data we will have at hand before making our predictions will essentially be all the values within the dataset except the columns associated with reviews and ratings. 

<br>

We will be using root mean squared error (RMSE) to evaluate the accuracy of our model's predictions. 

We decided to choose RMSE over other metrics because it is a straightforward variable.

- It is always expressed in the same units as our response variable so interpretation is simple. 
 
- It is sensitive to outliers so we can understand how our model handles inputs that are different from the norm of the training set.

<br>

---

<br>

## <strong> Baseline Model </strong> 

<br> 

We began our model development by choosing certain features to use. Our model utilized these columns as features:

1. <code> minutes </code> : time it takes to make the recipe

2. <code> tags </code> : characterstics / phrases associated with a recipe

3. <code> n_steps </code> : number of steps listed out in recipe

4. <code> nutrition </code> : list of health facts about recipe <br> &emsp;&emsp;&emsp;&emsp; - Includes calories, total fat, sugar, sodium, protein, saturated fat, and carbs.

5. <code> n_ingredients </code> : The number of ingredients required of recipe.

<br>

Of course we had to apply some feature engineering to these columns so they were usable by our decision tree regressor model. 

Our initial feature engineering was with <code>tags</code>. Each recipe contained a list of multiple tags that was very convoluted and messy. Thus, we decided to extract the first tag of each recipe's tag list and one hot encode these. At first we were unsure whether or not this was a valid way to associate a single tag. However, through some inspection we realized the tags were not ordered in any way so the first tag in the list must be the initial idea associated with a recipe and thus could be an optimal way to designate a single tag to a recipe. 

Furthermore, we applied feature engineering to <code>nutrition</code> -- which, to reiterate, was a list of health facts. We decided to apply a function transformer to this column which took the list and created 8 individual columns to store each health fact value individually. 

The rest of the columns were already in a numeric data type format that was usable by the model and we felt that there was nothing required of us to feature engineer further. 

In summary, we had <code>tags</code> as nominal categorical data that required a one hot encoding and we split the <code>nutrition</code> column into multiple columns of numeric values. The rest of our columns <code>minutes</code>, <code>n_steps</code>, and <code>n_ingredients</code> were already quantitative values and did not require any feature engineering. 


To explain our model further, we decided to utilize the <code>DecisionTreeRegressor</code> model of sklearn. We chose this model for the baseline because we knew we wanted our final model to be of the same class so it would be quick way for us to understand the effect of our chosen features on the development of our predictions without convoluting the pipeline or testing different hyperparameter combinations. In this way we saw that the testing dataset created through <code>train_test_split</code> had an RMSE of 0.949 on our testing set-- a great value that shows us that we are definitely not underfitting our model. 

For our baseline model we kept the default parameters offered by sklearn's DecisionTreeRegressor class:

- criterion: 'squared error'
- splitter: 'best'
- max_depth: None (expands nodes until all leaves are pure or less nodes than min_samples_split)
- min_samples_split: 2

<br>

---

<br>

## <strong> Final Model </strong> 

<br> 

The first step we took to develop our final prediction model was to apply more feature engineering. Thus we focused on the <code>minutes</code> column of the dataset. In our EDA (and for this project) we had decided to clean our dataset by removing any recipes that took over 24 hours to make. This made sense to us because if a recipe took over a day this most likely meant it was a more complicated recipe and thus only true cooking fanatics would take on this challenge while we wanted to maintain a sample of recipes and ratings that applied to a more general population of cooking hobbyists. 

For the <code>minutes</code> column, even though we had removed all the recipes over 24 hours long, there was still large variance in the dataset. Specifically, most of the recipes were under 2 hours (120 minutes) long, and our hypothesis from our EDA was that greater cooking times tend to have worse ratings. With this in my mind, we applied sklearn's <code>Binarizer</code> with the <code>threshold</code> of 120 (minutes). 

We also decided to standardize the columns <code>n_steps</code> and <code>n_ingredients</code>. The reason for standardizing these columns is because we felt that time was the biggest reflection of a good or bad rating. Although number of steps and ingredients does attribute to the time, it does not have a direct correlation and thus we did not want these columns to have too large of an effect on the predictions. 

We encoded <code>tags</code> the same as done before in the baseline model as done the same with the <code>nutrition</code> health facts. 


In the actual model, we decided to use a decision tree regressor to predict the average ratings for recipes. A decision tree regressor is a type of ML model that partitions inputs into smaller and smaller groupings so that it can create a prediction on a continuous quantiative value based off the input features. 

For our final model we chose these parameters:

- criterion: 'squared error'
- splitter: 'best'
- max_depth: 2
- min_samples_split: 5


With our final model we found an RMSE value on our testing set of 0.645. 

Comparing this RMSE with the RMSE of our previous baseline model of .949 it is obvious that our final model is an improves on the baseline model's accuracy with unseen data. 


<br>

---

<br>

## <strong> Fairness Analysis </strong> 

<br> 

For our fairness analysis, we decided to split our data into two groups, recipes with less than 10 ingredients and recipes that required at least 10 ingredients.

To evaluate the "fairness" between these two groups within our dataset, we will use the RMSE of our model's prediction. 

<strong>Null Hypothesis:</strong> Our model accurately evaluates the average rating for recipes with less than 10 ingredients and for recipes with at least 10 ingredients. 

<strong>Alternate Hypothesis:</strong> Our model's accuracy is less when evaluating recipes with less than 10 ingredients than for recipes with at least 10 ingredients. 

<strong>Test Statistic:</strong> The difference in RMSE of our model when predicting the average rating of recipes with less than 10 ingredients and recipes with at least 10 ingredients. 

<strong>Significance Level:</strong> 0.05 &emsp;&emsp; <strong>p-value:</strong> 0.41

<strong>Conclusion:</strong> With these values above for our significance level and p-value > .05, we fail to reject the null. There is insufficient evidence to state that our model performs worse for recipes with less than 10 ingredients.