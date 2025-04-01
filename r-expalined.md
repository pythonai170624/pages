# Understanding R² and Adjusted R² in Multiple Linear Regression

## What is R² (Coefficient of Determination)?

R² (pronounced "R-squared") is a statistical measure that represents the proportion of variance in the dependent variable that is explained by the independent variables in a regression model.

### Mathematical Definition

R² is defined as:

$$R^2 = 1 - \frac{SSE}{SST}$$

Where:
- **SSE** (Sum of Squared Errors) = $\sum_{i=1}^{n} (y_i - \hat{y}_i)^2$
  - This is the sum of squared differences between the observed values ($y_i$) and the predicted values ($\hat{y}_i$)
  - It represents the unexplained variance
  
- **SST** (Total Sum of Squares) = $\sum_{i=1}^{n} (y_i - \bar{y})^2$
  - This is the sum of squared differences between the observed values ($y_i$) and the mean of the observed values ($\bar{y}$)
  - It represents the total variance in the dependent variable

Alternatively, R² can be expressed as:

$$R^2 = \frac{SSR}{SST}$$

Where:
- **SSR** (Sum of Squares due to Regression) = $\sum_{i=1}^{n} (\hat{y}_i - \bar{y})^2$
- And SST = SSE + SSR

### Interpretation of R²

R² always falls between 0 and 1:
- **R² = 0**: The model explains none of the variability in the response data.
- **R² = 1**: The model explains all the variability in the response data.
- **R² = 0.75**: The model explains 75% of the variability in the response data.

In general:
- Higher R² values indicate a better fit of the model to the data.
- R² increases (or at least doesn't decrease) when you add more independent variables to the model, even if those variables don't actually improve the model's predictive power.

## What is Adjusted R²?

Adjusted R² modifies the R² value to account for the number of predictors in the model. It addresses a key weakness of R² - the fact that R² always increases when more variables are added to the model, even if those variables don't actually improve the model's predictive power.

### Mathematical Definition

Adjusted R² is defined as:

$$R^2_{adj} = 1 - \frac{(1-R^2)(n-1)}{n-p-1}$$

Where:
- $n$ is the sample size (number of observations)
- $p$ is the number of predictors (independent variables)

An alternative formula is:

$$R^2_{adj} = 1 - \frac{SSE/(n-p-1)}{SST/(n-1)}$$

### Why We Need Adjusted R²

We use adjusted R² to:

1. **Penalize Complexity**: It penalizes the addition of unnecessary predictors to the model.
2. **Prevent Overfitting**: It helps prevent the selection of overly complex models that fit the training data well but don't generalize to new data.
3. **Compare Models**: It allows for fair comparison between models with different numbers of predictors.

Unlike R², adjusted R² can decrease when you add variables that don't significantly improve the model's fit. This property makes it more useful for model selection.

## Understanding SST (Total Sum of Squares)

SST measures the total variability in the dependent variable:

$$SST = \sum_{i=1}^{n} (y_i - \bar{y})^2$$

It represents how much the actual values deviate from their mean. This can be thought of as the "variance that needs to be explained" by your regression model.

The breakdown of variance in regression is:
- **SST** (Total variance) = **SSR** (Explained variance) + **SSE** (Unexplained variance)

Where:
- SSR = $\sum_{i=1}^{n} (\hat{y}_i - \bar{y})^2$ (variation explained by the regression)
- SSE = $\sum_{i=1}^{n} (y_i - \hat{y}_i)^2$ (variation not explained by the regression)

## How to Use R² and Adjusted R² in Practice

### Model Selection

1. **Between Nested Models**: When choosing between nested models (where one model contains a subset of the predictors in another model), prefer the model with the higher adjusted R².

2. **General Model Selection**: When comparing multiple models:
   - Higher adjusted R² suggests a better balance of fit and simplicity
   - Look for the point where adding more variables no longer substantially increases adjusted R²

### Assessing Model Fit

1. **Absolute Assessment**: 
   - Very low R² (e.g., below 0.30) suggests the model is not capturing much of the variation in the data
   - Very high R² (e.g., above 0.95) in real-world data might suggest overfitting or potential data leakage

2. **Context-Dependent**: 
   - In social sciences, R² values of 0.3-0.5 might be considered good
   - In physical sciences, much higher R² values are typically expected
   - Time series forecasting may have lower acceptable R² values than cross-sectional studies

### Reporting in Research

When reporting results:
- Always report both R² and adjusted R² for transparency
- For multiple regression, adjusted R² is generally the more informative metric
- Consider reporting additional metrics like RMSE (Root Mean Square Error) for a more complete assessment

### Limitations to Keep in Mind

1. R² and adjusted R² don't tell you if:
   - The coefficient estimates are biased
   - You've chosen the right predictors
   - The relationship is causal rather than just correlational

2. A high R² doesn't necessarily mean the model is good if:
   - The model violates key assumptions (like linearity, independence, etc.)
   - The model doesn't generalize well to new data

3. R² should not be the only criterion for model selection:
   - Consider domain knowledge
   - Use cross-validation techniques
   - Look at prediction errors on test data

## Example: Interpreting R² and Adjusted R² in Apartment Price Prediction

Consider our apartment price prediction model:

```
Training R²: 0.98
Adjusted R²: 0.95
```

This means:
- 98% of the variance in apartment prices in our training data is explained by our four predictors (area, number of rooms, building age, and distance from city center)
- After adjusting for the number of predictors, the model explains 95% of the variance

These high values suggest the model fits the data very well. However, we should:
1. Validate on test data to ensure we're not overfitting
2. Check model assumptions (linearity, homoscedasticity, etc.)
3. Consider if these four predictors make sense from a domain knowledge perspective

## Conclusion

R² and adjusted R² are valuable tools for assessing how well your regression model fits the data and for comparing different models. While R² measures the proportion of variance explained by the model, adjusted R² provides a more nuanced measure that accounts for model complexity. Both metrics should be considered alongside other diagnostic tools when building and evaluating regression models.
