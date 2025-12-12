# What Makes a Recipe Great?
### Analyzing Factors Behind High Recipe Ratings  
**DSC 80 Final Project – UC San Diego**  
**Name:** Keith Gong

---

## Introduction

Online recipe platforms contain thousands of recipes, but not all are rated equally by users.  
This project analyzes the **Recipes and Ratings** dataset to understand:

> **What characteristics of a recipe are associated with higher average user ratings?**

Understanding these factors can help home cooks, content creators, and food platforms design better recipes and recommendations.

The dataset contains:
- **RAW_recipes.csv**: recipe metadata
- **RAW_interactions.csv**: user ratings

After cleaning and merging, the analysis focuses on:
- `rating`: average user rating per recipe
- `minutes`: cooking time
- `n_ingredients`: number of ingredients

---

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

To prepare the data:
- Removed interactions without ratings
- Computed **average rating per recipe**
- Merged interaction data with recipe metadata
- Removed recipes missing key fields (`minutes`, `n_ingredients`, `rating`)
- Filtered extreme cooking-time outliers (above the 99th percentile)

Below is the distribution of **average recipe ratings**.

<iframe
src="assets/ratings_histogram.html"
width="100%"
height="600"
frameborder="0"
></iframe>

**Observation:**  
Most recipes receive high average ratings, with a strong concentration between 4 and 5 stars.

---

### Univariate Analysis

#### Cooking Time Distribution

<iframe
src="assets/cook_time_histogram.html"
width="100%"
height="600"
frameborder="0"
></iframe>

**Observation:**  
Cooking times are heavily right-skewed, with most recipes requiring under one hour.

---

### Bivariate Analysis

#### Cooking Time vs Rating

<iframe
src="assets/cook_time_vs_rating.html"
width="100%"
height="600"
frameborder="0"
></iframe>

**Observation:**  
Ratings remain relatively consistent across cooking times, suggesting longer recipes are not necessarily rated lower.

---

#### Number of Ingredients vs Rating

<iframe
src="assets/ingredients_vs_rating.html"
width="100%"
height="600"
frameborder="0"
></iframe>

**Observation:**  
Recipes with a moderate number of ingredients tend to achieve slightly higher ratings than very simple or very complex recipes.

## Interesting Aggregates
<iframe
src="assets/ingredient_group_rating.html"
width="100%"
height="600"
frameborder="0"
></iframe>
**Observation:**  
Recipes with a medium to high number of ingredients tend to receive slightly higher average ratings, suggesting that some complexity is valued by users, while overly simple or overly complex recipes perform worse.
---

## Assessment of Missingness

### NMAR Analysis

The `rating` column is likely **NMAR (Not Missing At Random)** because users are more likely to leave ratings when they have strong positive or negative experiences. Additional data about user engagement would be required to fully model this missingness mechanism.

### Missingness Dependency

Permutation tests were conducted to analyze whether missing ratings depend on observable recipe attributes. Results indicate that missingness depends on user interaction behavior rather than recipe metadata alone.

---

## Hypothesis Testing

**Null Hypothesis:**  
There is no relationship between cooking time and average recipe rating.

**Alternative Hypothesis:**  
Cooking time is associated with differences in average recipe rating.

A permutation test using the difference in mean ratings was conducted at α = 0.05.  
The resulting p-value suggests insufficient evidence to reject the null hypothesis.

---

## Framing a Prediction Problem

**Prediction Task:**  
Predict a recipe’s average rating.

- **Type:** Regression
- **Response Variable:** `rating`
- **Evaluation Metric:** RMSE  
RMSE was chosen because it penalizes large prediction errors and is appropriate for continuous outcomes.

---

## Baseline Model

The baseline model uses:
- `minutes`
- `n_ingredients`

A linear regression model was trained using a single sklearn pipeline.  
The baseline model establishes a performance benchmark but shows limited predictive power.

---

## Final Model

The final model improves upon the baseline by:
- Log-transforming cooking time
- Engineering ingredient-count bins
- Tuning hyperparameters using cross-validation

The final model demonstrates improved RMSE relative to the baseline, indicating that feature engineering meaningfully improves predictive performance.

---

## Fairness Analysis

A fairness analysis was conducted by comparing model performance across groups defined by recipe complexity.  
A permutation test found no statistically significant performance disparity between groups, suggesting the model behaves fairly with respect to this attribute.

---

## Conclusion

This project demonstrates that while recipe ratings are generally high, factors such as ingredient count and preparation time still influence user perception.  
Future work could incorporate textual recipe features or user-level behavior to further improve predictions.
