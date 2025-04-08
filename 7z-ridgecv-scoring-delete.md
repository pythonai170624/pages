# RidgeCV and Scoring Metrics: Finding Optimal Regularization Parameters

## The Challenge: Selecting the Right Regularization Strength

When implementing Ridge regression, selecting the appropriate regularization parameter (λ or alpha) is critical to model performance. This parameter controls the trade-off between:

- **Model simplicity**: Higher values lead to more coefficient shrinkage
- **Training data fit**: Lower values allow the model to fit the training data more closely

Choosing λ manually is inefficient and often suboptimal. This is where `RidgeCV` becomes valuable.

## Understanding RidgeCV

`RidgeCV` is a built-in class in scikit-learn that performs Ridge regression with built-in cross-validation to select the optimal regularization parameter from a provided range of values.

### Basic Implementation

```python
from sklearn.linear_model import RidgeCV
import numpy as np

# Define a range of alphas to test
alphas = np.logspace(-2, 3, 100)  # 100 values from 0.01 to 1000

# Create RidgeCV model with 5-fold cross-validation
ridge_cv = RidgeCV(alphas=alphas, cv=5, scoring='neg_mean_squared_error')

# Train the model
ridge_cv.fit(X_train, y_train)

# Access the best alpha value selected by cross-validation
best_alpha = ridge_cv.alpha_
print(f"Best alpha: {best_alpha:.4f}")

# Make predictions using the best model
y_pred = ridge_cv.predict(X_test)
```

### How RidgeCV Works

1. For each alpha value in the provided range:
   - Perform k-fold cross-validation (default is 5-fold)
   - Train a Ridge model with the current alpha on each training fold
   - Evaluate the model on the corresponding validation fold
   - Calculate the average performance across all folds

2. Select the alpha value that results in the best average cross-validation performance

3. Train a final Ridge model on the entire training set using the best alpha

## Scoring Metrics: Evaluating Model Performance

The `scoring` parameter in `RidgeCV` determines which metric is used to evaluate model performance during cross-validation. Scikit-learn follows a "higher is better" convention, meaning the alpha that maximizes the scoring metric is selected.

### Common Scoring Metrics for Regression

| Metric | scikit-learn Score Name | Description | Formula |
|--------|-------------------------|-------------|---------|
| R² | `'r2'` | Coefficient of determination (proportion of variance explained) | $1 - \frac{\sum(y_i - \hat{y}_i)^2}{\sum(y_i - \bar{y})^2}$ |
| Neg. Mean Squared Error | `'neg_mean_squared_error'` | Negative of average squared errors | $-\frac{1}{n}\sum(y_i - \hat{y}_i)^2$ |
| Neg. Root MSE | `'neg_root_mean_squared_error'` | Negative of square root of average squared errors | $-\sqrt{\frac{1}{n}\sum(y_i - \hat{y}_i)^2}$ |
| Neg. Mean Absolute Error | `'neg_mean_absolute_error'` | Negative of average absolute errors | $-\frac{1}{n}\sum\|y_i - \hat{y}_i\|$ |
| Neg. Median Absolute Error | `'neg_median_absolute_error'` | Negative of median of absolute errors | $-\text{median}(\|y_i - \hat{y}_i\|)$ |

### Why Negative Error Metrics?

Scikit-learn's cross-validation functionality is designed so that **higher scoring values indicate better models**. Since error metrics (like MSE, RMSE, MAE) are naturally "lower is better" metrics, scikit-learn negates these values to maintain the "higher is better" convention.

For example:
- If a model has an MSE of 10, its `neg_mean_squared_error` score is -10
- If another model has an MSE of 5, its `neg_mean_squared_error` score is -5
- The second model has the higher (less negative) score, indicating better performance

This negation is simply a convention to ensure consistent behavior across different scoring metrics.

## Choosing the Right Scoring Metric

The choice of scoring metric should align with your project's goals and the nature of your data:

| Scoring Metric | When to Use |
|----------------|-------------|
| `'r2'` | When you want to measure the proportion of variance explained by the model |
| `'neg_mean_squared_error'` | When larger errors are more problematic than smaller ones (due to squaring); sensitive to outliers |
| `'neg_root_mean_squared_error'` | Similar to MSE but returns error in the same units as the target variable |
| `'neg_mean_absolute_error'` | When you want to weigh all errors equally; more robust to outliers |
| `'neg_median_absolute_error'` | When you want maximum robustness against outliers |

## Comprehensive Example: RidgeCV with Different Scoring Metrics

The following example demonstrates how different scoring metrics can lead to different optimal alpha values:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import make_regression
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import RidgeCV
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score

# Create synthetic data with outliers
np.random.seed(42)
X, y = make_regression(n_samples=100, n_features=10, noise=15, random_state=42)

# Add some outliers
y[np.random.choice(len(y), 5, replace=False)] *= 5

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Scale the features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Define range of alphas
alphas = np.logspace(-3, 3, 100)

# Create dictionary of scoring metrics to try
scoring_metrics = {
    'R²': 'r2',
    'Neg. MSE': 'neg_mean_squared_error',
    'Neg. RMSE': 'neg_root_mean_squared_error',
    'Neg. MAE': 'neg_mean_absolute_error',
    'Neg. Median AE': 'neg_median_absolute_error'
}

# Train RidgeCV with different scoring metrics
results = {}
for name, metric in scoring_metrics.items():
    ridge_cv = RidgeCV(alphas=alphas, cv=5, scoring=metric)
    ridge_cv.fit(X_train_scaled, y_train)
    
    # Store results
    y_pred = ridge_cv.predict(X_test_scaled)
    results[name] = {
        'best_alpha': ridge_cv.alpha_,
        'r2': r2_score(y_test, y_pred),
        'mse': mean_squared_error(y_test, y_pred),
        'rmse': np.sqrt(mean_squared_error(y_test, y_pred)),
        'mae': mean_absolute_error(y_test, y_pred)
    }

# Create a DataFrame from results
results_df = pd.DataFrame.from_dict(results, orient='index')
print(results_df)

# Plot the coefficients for different scoring metrics
plt.figure(figsize=(14, 8))
for i, (name, metric) in enumerate(scoring_metrics.items()):
    ridge_cv = RidgeCV(alphas=alphas, cv=5, scoring=metric)
    ridge_cv.fit(X_train_scaled, y_train)
    
    plt.subplot(2, 3, i+1)
    plt.bar(range(X.shape[1]), ridge_cv.coef_)
    plt.title(f"{name}\nBest Alpha: {ridge_cv.alpha_:.4f}")
    plt.xlabel('Feature')
    plt.ylabel('Coefficient')
    plt.grid(alpha=0.3)

plt.tight_layout()
plt.show()
```

## Practical Tips for Using RidgeCV

### 1. Choose an Appropriate Alpha Range

- Start with a wide range of alpha values (e.g., `np.logspace(-3, 3, 100)`)
- Focus on regions where performance changes significantly
- Refine your search if the optimal alpha is at either end of your range

```python
# Initial wide search
alphas_wide = np.logspace(-3, 3, 20)
ridge_cv = RidgeCV(alphas=alphas_wide, cv=5, scoring='neg_mean_squared_error')
ridge_cv.fit(X_train_scaled, y_train)
best_alpha_wide = ridge_cv.alpha_

# If best alpha is at the lower end of the range
if best_alpha_wide == min(alphas_wide):
    alphas_refined = np.logspace(-5, -2, 50)
    ridge_cv = RidgeCV(alphas=alphas_refined, cv=5, scoring='neg_mean_squared_error')
    ridge_cv.fit(X_train_scaled, y_train)
```

### 2. Visualize Alpha Selection

Visualizing model performance across different alpha values can provide insights into model stability:

```python
from sklearn.model_selection import cross_val_score

def plot_ridge_cv_path(X, y, alphas, cv=5, scoring='neg_mean_squared_error'):
    scores = []
    for alpha in alphas:
        ridge = Ridge(alpha=alpha)
        fold_scores = cross_val_score(ridge, X, y, cv=cv, scoring=scoring)
        scores.append(np.mean(fold_scores))
    
    plt.figure(figsize=(10, 6))
    plt.semilogx(alphas, scores)
    plt.xlabel('Alpha')
    plt.ylabel(f'Cross-validated {scoring}')
    plt.title('Ridge Regression Performance vs. Alpha')
    plt.axvline(alphas[np.argmax(scores)], color='r', linestyle='--',
                label=f'Best Alpha: {alphas[np.argmax(scores)]:.4f}')
    plt.legend()
    plt.grid(True)
    plt.show()
    
    return alphas[np.argmax(scores)]
```

### 3. Consider Nested Cross-Validation for Unbiased Evaluation

When you need a truly unbiased assessment of model performance:

```python
from sklearn.model_selection import GridSearchCV, KFold, cross_val_score

# Setup nested cross-validation
outer_cv = KFold(n_splits=5, shuffle=True, random_state=42)
inner_cv = KFold(n_splits=5, shuffle=True, random_state=42)

# Define Ridge with GridSearchCV for parameter tuning
param_grid = {'alpha': np.logspace(-3, 3, 20)}
ridge = Ridge()
grid_search = GridSearchCV(ridge, param_grid, cv=inner_cv, scoring='neg_mean_squared_error')

# Perform nested cross-validation
nested_scores = cross_val_score(grid_search, X, y, cv=outer_cv, scoring='neg_mean_squared_error')

print(f"Nested CV Average Score: {-np.mean(nested_scores):.4f}")
```

## Conclusion

`RidgeCV` combines the power of Ridge regression with the automation of hyperparameter selection, making it a robust and efficient approach to building regularized linear models. By understanding and choosing appropriate scoring metrics, you can tailor the optimization process to your specific needs, whether that's minimizing general prediction error, being robust against outliers, or maximizing the proportion of explained variance.

Remember that different scoring metrics may lead to different "optimal" alpha values, as they prioritize different aspects of model performance. The best approach is to align your choice of scoring metric with your project's goals and the specific characteristics of your data.
