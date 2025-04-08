# Assessing Linear Regression Suitability Through Residual Investigation

Linear regression is a powerful statistical method for modeling relationships between a dependent variable and one or more independent variables. However, not all datasets are suitable for linear regression. Residual analysis is one of the most effective ways to determine if your multiple-feature dataset can be appropriately modeled using linear regression.

## What Are Residuals?

Residuals are the differences between the observed values and the values predicted by the model:

$$\text{Residual}_i = y_i - \hat{y}_i$$

Where:
- $y_i$ is the actual observed value
- $\hat{y}_i$ is the predicted value from the model

## Key Assumptions of Linear Regression

For a linear regression model to be valid and reliable, several assumptions about the residuals should be met:

1. **Linearity**: The relationship between predictors and the response is linear
2. **Independence**: Residuals are independent of each other
3. **Homoscedasticity**: Residuals have constant variance across all levels of predictors
4. **Normality**: Residuals are normally distributed
5. **No multicollinearity**: Predictor variables are not highly correlated with each other

## Residual Investigation Techniques

### 1. Residuals vs. Fitted Values Plot

This plot helps check for linearity and homoscedasticity.

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.datasets import fetch_california_housing

# Load sample dataset
housing = fetch_california_housing()
X = pd.DataFrame(housing.data, columns=housing.feature_names)
y = housing.target

# Create and fit the model
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
model = LinearRegression()
model.fit(X_train, y_train)

# Make predictions and calculate residuals
y_pred = model.predict(X_train)
residuals = y_train - y_pred

# Create residuals vs. fitted values plot
plt.figure(figsize=(10, 6))
plt.scatter(y_pred, residuals, alpha=0.5)
plt.axhline(y=0, color='r', linestyle='-')
plt.xlabel('Fitted Values')
plt.ylabel('Residuals')
plt.title('Residuals vs. Fitted Values')

# Add a smoothed line to help visualize patterns
sns.regplot(x=y_pred, y=residuals, scatter=False, lowess=True, line_kws={'color': 'red'})

plt.grid(True, alpha=0.3)
plt.show()
```

**Interpretation:**
- **Ideal pattern**: Residuals should be randomly scattered around zero with no discernible pattern
- **Red flags**: 
  - Curved patterns indicate non-linearity
  - Funnel shapes indicate heteroscedasticity (non-constant variance)
  - Clusters suggest issues with model specification

### 2. Q-Q Plot for Normality

This plot helps check if residuals follow a normal distribution.

```python
import scipy.stats as stats

plt.figure(figsize=(10, 6))
stats.probplot(residuals, dist="norm", plot=plt)
plt.title('Q-Q Plot of Residuals')
plt.grid(True, alpha=0.3)
plt.show()
```

**Interpretation:**
- **Ideal pattern**: Points should closely follow the diagonal line
- **Red flags**:
  - S-shaped curves indicate skewness
  - Points deviating from the line at the ends suggest heavy or light tails

### 3. Scale-Location Plot (Spread-Location)

This plot specifically checks for homoscedasticity.

```python
plt.figure(figsize=(10, 6))
plt.scatter(y_pred, np.sqrt(np.abs(residuals)), alpha=0.5)
plt.xlabel('Fitted Values')
plt.ylabel('√|Standardized Residuals|')
plt.title('Scale-Location Plot')

# Add a smoothed line
sns.regplot(x=y_pred, y=np.sqrt(np.abs(residuals)), scatter=False, lowess=True, line_kws={'color': 'red'})

plt.grid(True, alpha=0.3)
plt.show()
```

**Interpretation:**
- **Ideal pattern**: Horizontal line with equally spread points
- **Red flags**: Funnel patterns or trends indicate heteroscedasticity

### 4. Residuals vs. Predictors Plots

These plots check if residuals have patterns related to specific predictors.

```python
# Create a multi-panel plot for each predictor
fig, axes = plt.subplots(2, 4, figsize=(20, 10))
axes = axes.flatten()

for i, column in enumerate(X_train.columns):
    if i < len(axes):
        axes[i].scatter(X_train[column], residuals, alpha=0.5)
        axes[i].axhline(y=0, color='r', linestyle='-')
        axes[i].set_xlabel(column)
        axes[i].set_ylabel('Residuals')
        axes[i].set_title(f'Residuals vs. {column}')
        axes[i].grid(True, alpha=0.3)
        
        # Add a smoothed line
        sns.regplot(x=X_train[column], y=residuals, ax=axes[i], scatter=False, 
                   lowess=True, line_kws={'color': 'red'})

plt.tight_layout()
plt.show()
```

**Interpretation:**
- **Ideal pattern**: No pattern in any of the plots
- **Red flags**: Any systematic patterns suggest relationships not captured by the model

### 5. Autocorrelation of Residuals

This helps check independence, especially for time series data.

```python
from statsmodels.graphics.tsaplots import plot_acf

plt.figure(figsize=(10, 6))
plot_acf(residuals, lags=20, alpha=0.05)
plt.title('Autocorrelation of Residuals')
plt.grid(True, alpha=0.3)
plt.show()
```

**Interpretation:**
- **Ideal pattern**: No significant autocorrelation (bars within confidence intervals)
- **Red flags**: Significant spikes indicate dependence between observations

### 6. Histogram of Residuals

This plot offers another way to check the normality assumption.

```python
plt.figure(figsize=(10, 6))
plt.hist(residuals, bins=30, edgecolor='black', alpha=0.7)
plt.xlabel('Residuals')
plt.ylabel('Frequency')
plt.title('Histogram of Residuals')

# Add a density curve
sns.kdeplot(residuals, color='red', label='Kernel Density Estimate')
# Add a normal distribution for comparison
x = np.linspace(min(residuals), max(residuals), 100)
plt.plot(x, stats.norm.pdf(x, np.mean(residuals), np.std(residuals)) * len(residuals) * (max(residuals) - min(residuals)) / 30,
         'g--', label='Normal Distribution')

plt.legend()
plt.grid(True, alpha=0.3)
plt.show()
```

**Interpretation:**
- **Ideal pattern**: Bell-shaped distribution centered at zero
- **Red flags**: Skewness, multiple peaks, or heavy tails

## Formal Statistical Tests

Beyond visual inspection, formal tests can help quantify the evidence for or against regression assumptions:

### 1. Shapiro-Wilk Test for Normality

```python
from scipy import stats

stat, p_value = stats.shapiro(residuals)
print(f"Shapiro-Wilk Test: Statistic={stat:.4f}, p-value={p_value:.4f}")
print(f"Interpretation: {'Residuals appear normal' if p_value > 0.05 else 'Residuals do not appear normal'}")
```

### 2. Breusch-Pagan Test for Homoscedasticity

```python
from statsmodels.stats.diagnostic import het_breuschpagan
import statsmodels.api as sm

# Add a constant for the intercept
X_train_const = sm.add_constant(X_train)
_, p_value, _, _ = het_breuschpagan(residuals, X_train_const)
print(f"Breusch-Pagan Test p-value: {p_value:.4f}")
print(f"Interpretation: {'Homoscedasticity assumption is met' if p_value > 0.05 else 'Heteroscedasticity detected'}")
```

### 3. Durbin-Watson Test for Autocorrelation

```python
from statsmodels.stats.stattools import durbin_watson

dw_statistic = durbin_watson(residuals)
print(f"Durbin-Watson Statistic: {dw_statistic:.4f}")
print("Interpretation:")
if dw_statistic < 1.5:
    print("Positive autocorrelation detected")
elif dw_statistic > 2.5:
    print("Negative autocorrelation detected")
else:
    print("No significant autocorrelation")
```

### 4. Variance Inflation Factor (VIF) for Multicollinearity

```python
from statsmodels.stats.outliers_influence import variance_inflation_factor

# Calculate VIF for each feature
vif_data = pd.DataFrame()
vif_data["Feature"] = X_train.columns
vif_data["VIF"] = [variance_inflation_factor(X_train.values, i) for i in range(X_train.shape[1])]

print("Variance Inflation Factors:")
print(vif_data)
print("Interpretation: VIF > 10 may indicate problematic multicollinearity")
```

## Addressing Issues Found in Residual Analysis

If your residual analysis reveals problems, consider these remedies:

1. **Non-linearity**: 
   - Transform variables (log, square root, etc.)
   - Add polynomial terms
   - Use splines or more flexible models

2. **Heteroscedasticity**:
   - Transform the response variable
   - Use weighted least squares
   - Use robust standard errors

3. **Non-normality**:
   - Transform the response variable
   - Use robust regression methods
   - Consider quantile regression

4. **Autocorrelation**:
   - Include lagged variables
   - Use time series models (ARIMA)
   - Use generalized least squares

5. **Multicollinearity**:
   - Remove redundant features
   - Use regularization (Ridge, Lasso)
   - Use dimensionality reduction techniques

## Complete Example: Assessing a Real Dataset

Here's a unified example that brings together all the residual investigation techniques:

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.datasets import fetch_california_housing
from sklearn.metrics import mean_squared_error, r2_score
import statsmodels.api as sm
from statsmodels.stats.diagnostic import het_breuschpagan
from statsmodels.stats.stattools import durbin_watson
from statsmodels.stats.outliers_influence import variance_inflation_factor
from scipy import stats
from statsmodels.graphics.tsaplots import plot_acf

# Set the aesthetic style
sns.set_style("whitegrid")
plt.rcParams['figure.figsize'] = [12, 8]

# Load the dataset
housing = fetch_california_housing()
X = pd.DataFrame(housing.data, columns=housing.feature_names)
y = housing.target

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Fit the model
model = LinearRegression()
model.fit(X_train, y_train)

# Make predictions
y_train_pred = model.predict(X_train)
y_test_pred = model.predict(X_test)

# Calculate residuals
residuals = y_train - y_train_pred

# Calculate performance metrics
train_mse = mean_squared_error(y_train, y_train_pred)
test_mse = mean_squared_error(y_test, y_test_pred)
train_r2 = r2_score(y_train, y_train_pred)
test_r2 = r2_score(y_test, y_test_pred)

print(f"Training MSE: {train_mse:.4f}, R²: {train_r2:.4f}")
print(f"Testing MSE: {test_mse:.4f}, R²: {test_r2:.4f}")

# Create a comprehensive residual analysis with multiple plots
fig = plt.figure(figsize=(15, 15))

# 1. Residuals vs Fitted
ax1 = fig.add_subplot(3, 2, 1)
ax1.scatter(y_train_pred, residuals, alpha=0.5)
ax1.axhline(y=0, color='r', linestyle='-')
sns.regplot(x=y_train_pred, y=residuals, ax=ax1, scatter=False, lowess=True, line_kws={'color': 'red'})
ax1.set_xlabel('Fitted Values')
ax1.set_ylabel('Residuals')
ax1.set_title('Residuals vs Fitted Values')

# 2. Q-Q Plot
ax2 = fig.add_subplot(3, 2, 2)
stats.probplot(residuals, dist="norm", plot=ax2)
ax2.set_title('Q-Q Plot of Residuals')

# 3. Scale-Location Plot
ax3 = fig.add_subplot(3, 2, 3)
ax3.scatter(y_train_pred, np.sqrt(np.abs(residuals)), alpha=0.5)
sns.regplot(x=y_train_pred, y=np.sqrt(np.abs(residuals)), ax=ax3, scatter=False, lowess=True, line_kws={'color': 'red'})
ax3.set_xlabel('Fitted Values')
ax3.set_ylabel('√|Residuals|')
ax3.set_title('Scale-Location Plot')

# 4. Residuals Histogram
ax4 = fig.add_subplot(3, 2, 4)
ax4.hist(residuals, bins=30, edgecolor='black', alpha=0.7)
sns.kdeplot(residuals, ax=ax4, color='red')
x = np.linspace(min(residuals), max(residuals), 100)
ax4.plot(x, stats.norm.pdf(x, np.mean(residuals), np.std(residuals)) * len(residuals) * (max(residuals) - min(residuals)) / 30,
       'g--', label='Normal')
ax4.legend()
ax4.set_xlabel('Residuals')
ax4.set_ylabel('Frequency')
ax4.set_title('Histogram of Residuals')

# 5. Autocorrelation Plot
ax5 = fig.add_subplot(3, 2, 5)
plot_acf(residuals, lags=20, alpha=0.05, ax=ax5)
ax5.set_title('Autocorrelation of Residuals')

# 6. Residuals vs Index (for any time-related patterns)
ax6 = fig.add_subplot(3, 2, 6)
ax6.scatter(range(len(residuals)), residuals, alpha=0.5)
ax6.axhline(y=0, color='r', linestyle='-')
sns.regplot(x=range(len(residuals)), y=residuals, ax=ax6, scatter=False, lowess=True, line_kws={'color': 'red'})
ax6.set_xlabel('Index')
ax6.set_ylabel('Residuals')
ax6.set_title('Residuals vs Index')

plt.tight_layout()
plt.show()

# Statistical tests for regression assumptions
print("\n--- Statistical Tests for Regression Assumptions ---\n")

# Normality test
stat, p_value = stats.shapiro(residuals)
print(f"Shapiro-Wilk Test for Normality: Statistic={stat:.4f}, p-value={p_value:.6f}")
print(f"Interpretation: {'Residuals appear normal' if p_value > 0.05 else 'Residuals do not appear normal'}")

# Homoscedasticity test
X_train_const = sm.add_constant(X_train)
_, p_value, _, _ = het_breuschpagan(residuals, X_train_const)
print(f"\nBreusch-Pagan Test for Homoscedasticity: p-value={p_value:.6f}")
print(f"Interpretation: {'Homoscedasticity assumption is met' if p_value > 0.05 else 'Heteroscedasticity detected'}")

# Autocorrelation test
dw_statistic = durbin_watson(residuals)
print(f"\nDurbin-Watson Test for Autocorrelation: {dw_statistic:.4f}")
if dw_statistic < 1.5:
    interpretation = "Positive autocorrelation detected"
elif dw_statistic > 2.5:
    interpretation = "Negative autocorrelation detected"
else:
    interpretation = "No significant autocorrelation"
print(f"Interpretation: {interpretation}")

# Multicollinearity test
print("\nVariance Inflation Factors for Multicollinearity:")
vif_data = pd.DataFrame()
vif_data["Feature"] = X_train.columns
vif_data["VIF"] = [variance_inflation_factor(X_train.values, i) for i in range(X_train.shape[1])]
print(vif_data)
```

## Conclusion

Residual investigation is an essential step in assessing whether linear regression is appropriate for your dataset. By examining residual plots and conducting statistical tests, you can identify violations of linear regression assumptions and take appropriate corrective actions.

Remember that perfectly meeting all assumptions is rare in real-world data. The goal is to identify serious violations that might affect your model's reliability and take corrective action as needed. Minor deviations from assumptions might be acceptable depending on your analysis goals.

## References

1. Fox, J. (2015). *Applied Regression Analysis and Generalized Linear Models*. Sage Publications.
2. Kutner, M.H., Nachtsheim, C.J., Neter, J., & Li, W. (2004). *Applied Linear Statistical Models*. McGraw-Hill/Irwin.
3. James, G., Witten, D., Hastie, T., & Tibshirani, R. (2013). *An Introduction to Statistical Learning*. Springer.
