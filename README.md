# RecipeRatingPredictions

### <u> Overview</u>: 

Previously we completed an EDA (exploratory data analysis on a recipe rating dataset from [food.com](https://dsc80.com/project3/recipes-and-ratings/food.com). This analysis can be found [here](https://akar247.github.io/RecipesDurationAnalysis/). Through this website we have documented our process for creating predictions on the dataset. 

<br>

---


<br>


## Framing the Problem
<br>

With this dataset that contains information such as the recipe name, recipe cooking time, certain tags associated with the recipe, steps, ingredients, etc, we hope to utilize certain features to create a regression model on the average rating on a recipe. 

<br>

Specifically, the response variable we are predicting is the average rating associate with each recipe given certain features. The data we will have at hand before making our predictions will essentially be all the values within the dataset except the columns associated with reviews and ratings. 

<br>

We will be using root mean squared error (rmse) to evaluate the accuracy of our model's predictions. 

We decided to choose rmse over other metrics because it is a straightforward variable.

- It is always expressed in the same units as our response variable to interpretation is simple. 
 
- It is sensitive to outliers so we can understand how our model handles inputs that are much different from the norm of the training set.

<br>

---

<br>

## Baseline Model

<br> 

We began our model development by choosing certain features to use. Our model utilized these columns as features:

1. <code> minutes </code> : time it takes to make the recipe

2. <code> tags </code> : characterstics / phrases associated with a recipe

3. <code> n_steps </code> : number of steps listed out in recipe

4. <code> nutrition </code> : list of health facts about recipe <br> &emsp;&emsp;&emsp;&emsp; - Includes calories, total fat, sugar, sodium, protein, saturated fat, and carbs.

5. <code> n_ingredients </code> : The number of ingredients required of recipe.

<br>

Of course we had to apply some feature engineering to these columns so they were usable by our decision tree regressor model. 

Our initial feature engineering was with tags. Each recipe contained a list of multiple tags that was very convoluted and messy. Thus, we decided to extract the first tag of each recipe's tag list and one hot encode these. At first we were unsure whether or not this was a valid way to associate a single tag. However, through some inspection we realized the tags were not ordered in any way so the first tag in the list must be the initial idea associated with a recipe and thus could be an optimal way to designate a single tag to a recipe. 

