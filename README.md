# RecipeRatingPredictions

### <u> Overview: </u>

Previously we completed an EDA (exploratory data analysis on a recipe rating dataset from [food.com](https://dsc80.com/project3/recipes-and-ratings/food.com). This analysis can be found [here](https://akar247.github.io/RecipesDurationAnalysis/). Through this website we have documented our process for creating predictions on the dataset. 

<br>

---


<br>


## Framing the Problem

<br>

With this dataset that contains information such as the recipe name, recipe cooking time, certain tags associated with the recipe, steps, ingredients, etc, we hope to utilize certain features to create a regression model on the average rating on a recipe. 


<br>

Specifically, the response variable we are predicting is the average rating associate with each recipe given certain features. The data we will have at hand before making our predictions will be: 

1. <code> minutes </code> : time it takes to make the recipe

2. <code> n_steps </code> : number of steps listed out in recipe

3. <code> nutrition </code> : health facts about recipe 

&emsp;&emsp;&emsp;&emsp;includes calories, total fat, sugar, sodium, protein, saturated fat, and carbs

4. <code> n_ingredients </code>


We will be using root mean squared error (rmse) to evaluate the accuracy of our model's predictions. 

We decided to choose rmse over other metrics because it is a straightforward variable.

- It is always expressed in the same units as our response variable to interpretation is simple. 
 
- It is sensitive to outliers so we can understand how our model handles inputs that are much different from the norm of the training set.



