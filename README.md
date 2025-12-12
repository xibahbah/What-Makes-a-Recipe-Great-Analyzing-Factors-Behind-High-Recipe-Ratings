# What Makes a Recipe Great?
### Analyzing Factors Behind High Recipe Ratings  
**DSC 80 Final Project – UC San Diego**  
**Name:** Keith Gong

---

## Introduction

Online recipe platforms host thousands of user-submitted recipes, each receiving feedback in the form of ratings. While most recipes receive generally positive reviews, there is meaningful variation in how highly recipes are rated. This project investigates the **Recipes and Ratings** dataset to answer the following question:

**What characteristics of a recipe are associated with higher average user ratings?**

Understanding these relationships can help home cooks design better recipes, assist content creators in optimizing engagement, and support platforms in improving recommendation systems.

This project uses two datasets:
- **RAW_recipes.csv**, which contains recipe-level metadata.
- **RAW_interactions.csv**, which contains user ratings.

After cleaning and merging the datasets, the analysis focuses on:
- `rating`: average user rating per recipe  
- `minutes`: total cooking time  
- `n_ingredients`: number of ingredients  

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

The following cleaning steps were performed to ensure valid analysis:
- Removed interactions without ratings.
- Computed average rating per recipe.
- Merged interaction data with recipe metadata.
- Removed recipes missing key variables (`minutes`, `n_ingredients`, or `rating`).
- Filtered extreme cooking-time outliers by removing values above the 99th percentile.

These steps reflect the data-generating process and prevent extreme or incomplete observations from distorting results.

---

### Univariate Analysis

#### Distribution of Average Recipe Ratings

<iframe
src="assets/ratings_histogram.html"
width="100%"
height="600"
frameborder="0"
></iframe>

Most recipes receive high ratings, with a strong concentration between 4 and 5 stars. This motivates examining subtler factors that differentiate highly rated recipes.

---

#### Distribution of Cooking Time

<iframe
src="assets/cook_time_histogram.html"
width="100%"
height="600"
frameborder="0"
></iframe>

Cooking time is heavily right-skewed, with most recipes requiring under one hour and a small number taking significantly longer.

---

### Bivariate Analysis

#### Cooking Time vs Average Rating

<iframe
src="assets/cook_time_vs_rating.html"
width="100%"
height="600"
frameborder="0"
></iframe>

Ratings remain relatively stable across cooking times, suggesting longer recipes are not systematically rated higher or lower.

---

#### Number of Ingredients vs Average Rating

<iframe
src="assets/ingredients_vs_rating.html"
width="100%"
height="600"
frameborder="0"
></iframe>

Recipes with a moderate number of ingredients tend to receive slightly higher ratings than very simple or very complex recipes.

---

### Interesting Aggregates

<iframe
src="assets/ingredient_group_rating.html"
width="100%"
height="600"
frameborder="0"
></iframe>

Grouped averages show that recipes with medium-to-high ingredient counts receive marginally higher ratings, suggesting users value some complexity without excessive difficulty.

---

## Assessment of Missingness

### NMAR Analysis

The `rating` column is likely **NMAR (Not Missing At Random)**. Users are more likely to submit ratings when they have strong opinions about a recipe. Additional user-level engagement data would be required to model this missingness as MAR.

---

### Missingness Dependency

Permutation tests indicate that missing ratings are not strongly dependent on observable recipe attributes such as cooking time or ingredient count, suggesting missingness is driven primarily by user behavior.

---

## Hypothesis Testing

**Null Hypothesis:**  
There is no association between cooking time and average recipe rating.

**Alternative Hypothesis:**  
Average recipe ratings differ across cooking times.

A permutation test using the difference in mean ratings was conducted at α = 0.05. The resulting p-value provides insufficient evidence to reject the null hypothesis.

---

## Framing a Prediction Problem

**Prediction Task:** Predict a recipe’s average rating.

- **Type:** Regression  
- **Response Variable:** `rating`  
- **Evaluation Metric:** RMSE  

Only features available at the time a recipe is published are used, ensuring a realistic prediction scenario.

---

## Baseline Model

The baseline model uses:
- `minutes`
- `n_ingredients`

A linear regression model implemented in a single sklearn pipeline establishes a reference performance level but shows limited predictive power.

---

## Final Model

The final model improves upon the baseline by:
- Log-transforming cooking time.
- Engineering ingredient-count bins.
- Tuning hyperparameters using cross-validation.

These changes lead to improved RMSE and better reflect the data-generating process.

---

## Fairness Analysis

Model performance was compared across groups defined by recipe complexity using RMSE. A permutation test found no statistically significant difference in performance, suggesting the model behaves fairly with respect to this attribute.

---

## Conclusion

Recipe ratings are generally high, but ingredient count shows a subtle relationship with user satisfaction. Cooking time alone does not strongly influence ratings. Future work could incorporate textual recipe features or user review data to further improve predictions.
