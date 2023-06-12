# # Prediction By Exploring Recipes and Ratings

**Name(s)**: Nahuel Canavy

## Introduction:

We're looking at a dataset from Food.com, a large recipe-sharing site. This dataset has lots of information about recipes and people's reactions to them. I've aleady done some analysis on this dataset too see if recipes with more calories get lower rating.

However, today we're going to tackle a new subject.

## Framing the Problem

**Prediction Problem:** Regression

**Response Variable:** Number of Steps

**Metric:** Mean Square Error and R2 Score

**Explanation:**
In this problem, the goal is to predict the number of steps using various features from the dataset. The evaluation of the regression model's performance can be done using Mean Square Error (MSE) as the metric. MSE measures the average difference between predicted and true values, providing an indication of prediction error magnitude. Additionally, R-squared can be used as it measures the proportion of variance in the number of steps that can be explained by the predictors. A high R-squared value suggests a good fit, while a low value indicates a poor fit. Combining MSE and R-squared provides a comprehensive understanding of the model's accuracy and explanatory power in predicting the number of steps.

### Dataset Info:

Our dataset has many rows, 83782 exaclty, each for a different recipe. The important columns for our question are:

### Here is an overview of our dataset.

| name                               |     id | minutes | contributor_id | submitted  | tags                                                                                                                                                                                                                                                                                               | nutrition                                     | n_steps | steps                                                                                                                                                                                                                                                 | description                                                                                                                                                                                                                                                          | ingredients                                                                                                                                                                    | n_ingredients | rating |
| :--------------------------------- | -----: | ------: | -------------: | :--------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------- | ------: | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------: | -----: |
| 1 brownies in the world best ever  | 333281 |      40 |         985201 | 2008-10-27 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings']                                                                        | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]      |      10 | ['heat the oven to 350f and arrange the rack in the middle' (...)']                                                                                                                                                                                   | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven! | ['bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour'] |             9 |      4 |
| 1 in canada chocolate chip cookies | 453467 |      45 |        1848091 | 2011-04-11 | ['60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings']                                                                                                                                      | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]  |      12 | ['pre-heat oven the 350 degrees f', 'in a mixing bowl , sift together the flours and baking powder', 'set aside', 'in another mixing bowl (...)]                                                                                                      | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                               | ['white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips']                    |            11 |      5 |
| 412 broccoli casserole             | 306168 |      40 |          50969 | 2008-05-30 | ['60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli']                                                                                                                                               | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]     |       6 | ['preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray , set aside'(...)]                                                                                                                                                    | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i (...)']                                                                                                                                                                               | 9                                                                                                                                                                              |             5 |
| millionaire pound cake             | 286009 |     120 |         461724 | 2008-02-12 | ['time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'north-american', 'desserts', 'american', 'southern-united-states', 'dinner-party', 'holiday-event', 'cakes', 'dietary', 'christmas', 'thanksgiving', 'low-sodium', 'low-in-something', 'taste-mood', 'sweet', '4-hours-or-less'] | [878.3, 63.0, 326.0, 13.0, 20.0, 123.0, 39.0] |       7 | ['freheat the oven to 300 degrees', (...)                                                                                                                                                                                                             | ['butter', 'sugar', 'eggs', 'all-purpose flour', 'whole milk', 'pure vanilla extract', 'almond extract']                                                                                                                                                             | 7                                                                                                                                                                              |             5 |
| 2000 meatloaf                      | 475785 |      90 |        2202916 | 2012-03-06 | ['time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'potatoes', 'vegetables', '4-hours-or-less', 'meatloaf', 'simply-potatoes2']                                                                                                                                             | [267.0, 30.0, 12.0, 12.0, 29.0, 48.0, 2.0]    |      17 | ['pan fry bacon , and set aside on a paper towel to absorb excess grease', 'mince yellow onion , red bell pepper , and add to your mixing bowl', 'chop garlic and set aside', 'put 1tbsp olive oil into a saut pan , along with chopped garlic (...)] | ['meatloaf mixture', 'unsmoked bacon', 'goat cheese', 'unsalted butter', 'eggs', 'baby spinach', 'yellow onion', 'red bell pepper', 'simply potatoes shredded hash browns', 'fresh garlic', 'kosher salt', 'white pepper', 'olive oil']                              | 13                                                                                                                                                                             |             5 |

## Cleaning and preparing the data

#### Data Cleaning like in project 3

Let's clean the data appropriately.
First, let's remove rows where rating or nutrition information is missing
Then, we'll split the nutrition information into separate columns and convert them from object to float.

#### More data cleaning

I need to clean furthermore my data. I'm going to drop columns that I think are not related to predict the numbers of steps
like name,id,contributor_id,submitted, tags and rating.
Furthermore, I need to convert columns of string data to numbers so my models can understand the data.
I'm going to transform the step,description and ingredients columns to the numbers of words they contains.

Finally, we've let with those datas :

|                   | 0       |
| :---------------- | :------ |
| minutes           | int64   |
| n_steps           | int64   |
| n_ingredients     | int64   |
| rating            | float64 |
| calories          | float64 |
| steps_words       | int64   |
| description_words | int64   |
| ingredients_words | int64   |

I think that every columns might have an impact here on the numbers of steps. And here is the dataframe head :

   minutes |   n_steps |   n_ingredients |   rating |   steps_words |   description_words |   ingredients_words |\n|----------:|----------:|----------------:|---------:|--------------:|--------------------:|--------------------:|\n|        40 |        10 |               9 |        4 |           128 |                  41 |                  18 |\n|        45 |        12 |              11 |        5 |           148 |                  42 |                  18 |\n|        40 |         6 |               9 |        5 |            92 |                  64 |                  21 |\n|       120 |         7 |               7 |        5 |           128 |                  34 |                  12 |\n|        90 |        17 |              13 |        5 |           257 |                  29 |                  29 |
### Baseline Model

So, the baseline model is a regression model that predicts the number of steps. It uses the features 'minutes' and 'n_ingredients'.

The model's performance is evaluated using Mean Squared Error (MSE) and R2 Score.

After building it, here are the result :

Mean Squared Error: 31.16342051823775
R2 Score: 0.18450057434464318

Here is the dataframe head :

|   minutes |   n_ingredients |   n_steps |   predicted_n_steps |\n|----------:|----------------:|----------:|--------------------:|\n|       495 |              15 |         9 |             14.2936 |\n|        25 |               9 |        10 |              9.9304 |\n|        35 |              12 |        11 |             12.1088 |\n|        75 |              21 |        22 |             18.6441 |\n|       170 |              15 |        38 |             14.289  |

And here is a plot of all the predict steps versus the actual numbers of steps :

<iframe src="assets/Predicted_vs_Actual_Number_of_Steps_baseline.html" width=800 height=600 frameBorder=0></iframe>

And here is a plot of the residual :

<iframe src="assets/Residual_Plot_baseline.html" width=800 height=600 frameBorder=0></iframe>

With the following precision :

Precision at 90%: 8.108563273527166
Precision at 95%: 10.43849664191145
Precision at 99%: 18.44764004730724

We can see that the baseline model is well pretty bad but it's only a baseline model and we must improve on it.

#### Final Model - V1

Lets do a first final model. We're going to still use a LinearRegression but will all the data disponible this time. Furthermore, because we know that our numbers of steps is a round numbers, we're going to round the result.

I think that the three features that seems really important to me that are the numbers of words in the steps, description and ingredients. I think the longueur the description, the longueur the recipe is usually. So, this time, we are going to use every features of our dataframe.

Here how it performed :

Mean Squared Error: 8.808229813026589
R2 Score: 0.7843539114289183

Here is the dataframe head :

|   minutes |   n_ingredients |   rating |   steps_words |   description_words |   ingredients_words |   n_steps |   predicted_n_steps |\n|----------:|----------------:|---------:|--------------:|--------------------:|--------------------:|----------:|--------------------:|\n|         1 |               2 |      4.5 |             5 |                   9 |                   3 |         1 |                   2 |\n|        70 |              15 |      1   |            94 |                  37 |                  21 |         8 |                  10 |\n|        10 |               3 |      5   |            28 |                  22 |                   3 |         3 |                   4 |\n|        25 |              11 |      2.5 |           114 |                  31 |                  22 |         8 |                  11 |\n|        40 |               7 |      5   |            63 |                  28 |                  11 |         4 |                   7 |

And here is a plot of all the predict steps versus the actual numbers of steps :

<iframe src="assets/Predicted_vs_Actual_Number_of_Steps_final_model_1.html" width=800 height=600 frameBorder=0></iframe>

And here is a plot of the residual :

<iframe src="assets/Residual_Plot_baseline_final_model_1.html.html" width=800 height=600 frameBorder=0></iframe>

With the following precision :

Precision at 90%: 4.0
Precision at 95%: 6.0
Precision at 99%: 10.0

As we can see, we have way better result result than the baseline model with all the others columns added. However, we're using a simple model that is a linear regression. Maybe a more sophisticated model could be better.

#### Final Model - V2

This model uses RandomForestRegressor as the algorithm and includes all columns except for 'n_steps' as features for prediction. The hyperparameters 'max_depth' and 'n_estimators' are set to specific values (10 and 200, respectively) after doing a GridSearch to find the bests. The model's performance is still evaluated using metrics such as Mean Squared Error (MSE) and R2 Score.

Here how it performed :

Mean Squared Error: 8.883265989947768
R2 Score: 0.783468767587091

Here is the dataframe head :

|   minutes |   n_ingredients |   rating |   steps_words |   description_words |   ingredients_words |   n_steps |   predicted_n_steps |\n|----------:|----------------:|---------:|--------------:|--------------------:|--------------------:|----------:|--------------------:|\n|        13 |               8 |  5       |            63 |                  41 |                  17 |         5 |                   7 |\n|       135 |              12 |  5       |           198 |                  49 |                  17 |        14 |                  17 |\n|        70 |               9 |  4       |           171 |                   3 |                  21 |        22 |                  16 |\n|        35 |              12 |  5       |           131 |                  84 |                  25 |        13 |                  12 |\n|        25 |               4 |  4.90909 |            32 |                  12 |                   7 |         4 |                   4 |

And here is a plot of all the predict steps versus the actual numbers of steps :

<iframe src="assets/Predicted_vs_Actual_Number_of_Steps_final_model_2.html" width=800 height=600 frameBorder=0></iframe>

And here is a plot of the residual :

<iframe src="assets/Residual_Plot_baseline_final_model2.html" width=800 height=600 frameBorder=0></iframe>

With the following precision :

Precision at 90%: 5.0
Precision at 95%: 6.0
Precision at 99%: 10.0

As we can see, even if we have a more complicated model, the precision is more off, only by a few still when we look at the R2 score and the MSE differences. Therefore, I decided to thune a bit this last model.

### Final Model - V2 Thune

So, I decided to add some features engineering by adding a feature 'total_words' which sum the total words of steps_words,description_words and ingredients_words. This feature might indicate better the relation beetween a long use of words and the numbers of steps since I think when there is a lot a step, there is a lot of words also. I also added a StandardScaler to standardize the feature values. I then did again a GridSearch to find the best hyperparameters that are 10 for the max_depth and 300 for the n_estimators. The model's performance is still evaluated using metrics such as Mean Squared Error (MSE) and R2 Score.

Here how it performed :

Mean Squared Error: 8.458509904405243
R2 Score: 0.792055755312701

Here is the dataframe head :

|   minutes |   n_ingredients |   rating |   steps_words |   description_words |   ingredients_words |   total_words |   n_steps |   predicted_n_steps |\n|----------:|----------------:|---------:|--------------:|--------------------:|--------------------:|--------------:|----------:|--------------------:|\n|        20 |               6 |  4.66667 |            52 |                  40 |                   8 |           100 |         7 |                   6 |\n|        50 |              10 |  4       |           102 |                  21 |                  30 |           153 |        13 |                  10 |\n|        35 |               8 |  5       |            92 |                  70 |                  13 |           175 |        11 |                   9 |\n|        40 |              18 |  5       |            86 |                  26 |                  37 |           149 |         9 |                   8 |\n|        10 |               5 |  5       |            92 |                  77 |                  11 |           180 |         6 |                   8 |

And here is a plot of all the predict steps versus the actual numbers of steps :

<iframe src="assets/Predicted_vs_Actual_Number_of_Steps_final_model_2t.html" width=800 height=600 frameBorder=0></iframe>

And here is a plot of the residual :

<iframe src="assets/Residual_Plot_baseline_final_model2t.html" width=800 height=600 frameBorder=0></iframe>

With the following precision :

Precision at 90%: 4.0
Precision at 95%: 6.0
Precision at 99%: 10.0

So, this model performed better than the previous one but still only by a few. However, compared to the baseline model, it performed way better. Therefore, I will stay with this final model. Let's now perform a fairness analysis.

## Fairness Analysis

For our fairness analysis, let's take two categories of recipes such as simple recipes that take than 10 ingredients and complex recipes that take 10 or more ingredients.

Let's define the null and alternative hypotheses:

Null Hypothesis: The model's mean squared error (MSE) or R2 score is the same for both simple recipes and complex recipes.

Alternative Hypothesis: The model's MSE or R2 score is different for simple recipes and complex recipes.

So, after dividing in two groups and calculating the observed scores, here are the values :

The absolute r2 difference is: 0.030291847744350675
The absolute mse difference is: 6.48260169085053
The percentage r2 difference is: 3.8714647336710275 %.
The percentage mse difference is: 114.83283054389571 %.

These values are really hight, the difference is really big. However, this was maybe predictible. Indeed, we can say that the more complex a recipe is, the more steps it would require :

<iframe src="assets/Distribution_of_Steps_for_Simple_vs_Complex_Recipes.html" width=800 height=600 frameBorder=0></iframe>

And as we could see in our previous plot, the model is less precise for recipe with a high number of steps.

### Permutation Test

Let's now perform a permutation test with a numbers of permutation of 1000.

After doind it, here is our result :

<iframe src="assets/Permutation_TesT_R2_Score_Difference.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/Permutation_TesT_MSE_Score_Difference.html" width=800 height=600 frameBorder=0></iframe>

And the values associated :

P-value for R2 score difference: 0.001
P-value for MSE difference: 0.0

Well, the p-values for both R2 score difference and MSE difference are significantly less than 0.05.

Thus, we reject the null hypothesis in favor of the alternative hypothesis. This suggests that the model's performance, as measured by both MSE and R2 score, is significantly different for simple recipes than it is for complex recipes.

Therefore, these results do suggest that the complexity of the recipe, as measured by the number of ingredients, is an important factor in the model's performance.

## Conclusion

In this project, we aimed to predict the number of steps in a recipe using a dataset from Food.com. The dataset, consisting of various features such as recipe duration, ingredients, and their descriptions, was cleaned and analyzed.

We initially built a baseline model using the features 'minutes' and 'n_ingredients'. It had an MSE of 31.16 and an R2 score of 0.18. It wasn't effective, and the model's performance was relatively poor.

For improvement, we constructed multiple models. Our first final model incorporated all features, including the number of words in steps, description, and ingredients, which we believed were critical. The model was a linear regression and performed better than the baseline, reducing the MSE to 8.80 and increasing the R2 score to 0.78.

We then employed a RandomForestRegressor model, optimizing it through GridSearch and adding feature engineering techniques. This model showed an improvement, reducing the MSE to 8.46 and increasing the R2 score to 0.79. It suggests that a more sophisticated model combined with appropriate feature engineering can provide better performance.

However, the model's fairness analysis revealed that its performance varies based on recipe complexity. The MSE and R2 score were significantly different for simple and complex recipes. A permutation test confirmed this difference was statistically significant. Hence, recipe complexity significantly influences model performance, and improvements are needed to make predictions more consistent across different types of recipes . 
