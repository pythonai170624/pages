# שימוש ב-Pairplot, פיצול נתונים, ורגרסיה לינארית

## 1. למה להשתמש ב-Pairplot לבדיקת פרמטרים?

### מהו Pairplot?
`pairplot` הוא כלי ויזואליזציה מספריית Seaborn המאפשר לנו לראות במבט אחד את מערכת היחסים בין כל הזוגות האפשריים של משתנים במערך נתונים.

### יתרונות השימוש ב-Pairplot
- **זיהוי מהיר של קשרים**: מציג את כל היחסים בין המשתנים בבת אחת
- **איתור דפוסים**: מאפשר לזהות מגמות לינאריות או לא-לינאריות בין משתנים
- **זיהוי מתאמים**: עוזר לראות אילו משתנים מתואמים באופן חיובי או שלילי
- **איתור חריגים**: מאפשר לראות נקודות נתונים חריגות בכמה ממדים
- **יעילות בזמן**: חוסך את הצורך ליצור גרפים נפרדים לכל זוג משתנים

### דוגמה בסיסית לשימוש ב-Pairplot

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd

# Sample DataFrame with multiple columns
df = pd.DataFrame({
    'var1': [1, 2, 3, 4, 5],
    'var2': [5, 4, 3, 2, 1],
    'var3': [2, 3, 4, 5, 6],
    'group': ['A', 'A', 'B', 'B', 'C']
})

# Create a basic pairplot
sns.pairplot(df)
plt.show()

# Create a pairplot with coloring by group
sns.pairplot(df, hue='group')
plt.show()
```

### דוגמה מתקדמת - ניתוח מערך נתונים אמיתי

```python
import seaborn as sns
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np

# Load the iris dataset as an example
iris = sns.load_dataset('iris')

# Create a pairplot with customized parameters
sns.pairplot(iris, 
             hue='species',                  # Color by species
             diag_kind='kde',                # Show KDE plot on diagonal
             plot_kws={'alpha': 0.6},        # Set transparency for scatter plots
             height=2.5,                     # Set subplot size
             markers=['o', 's', 'D'])        # Different markers for each species
plt.suptitle('Iris Dataset Feature Relationships', y=1.02)
plt.show()

# Calculate and visualize correlation matrix as a heatmap
correlation_matrix = iris.drop('species', axis=1).corr()
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Correlation Matrix of Iris Features')
plt.show()
```

## 2. פיצול נתונים באמצעות `train_test_split`

### למה לפצל נתונים?
פיצול נתונים למערכי אימון ובדיקה חיוני כדי:
- למנוע התאמת יתר (overfitting)
- להעריך את ביצועי המודל על נתונים חדשים
- לבדוק את יכולת ההכללה של המודל

### דוגמת שימוש

```python
from sklearn.model_selection import train_test_split
import numpy as np
import pandas as pd

# Example data
X = np.array([[1, 2], [3, 4], [5, 6], [7, 8], [9, 10], [11, 12]])  # features
y = np.array([0, 0, 1, 1, 2, 2])  # labels

# Split data into 70% training and 30% testing
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.3, random_state=42)

print("X training set:", X_train)
print("X testing set:", X_test)
print("y training labels:", y_train)
print("y testing labels:", y_test)
```

### פרמטרים חשובים ב-`train_test_split`
- **test_size**: החלק היחסי של הנתונים להקצאה למערך הבדיקה (בין 0 ל-1)
- **random_state**: מספר שמבטיח שתקבלו תוצאות זהות בכל ריצה
- **stratify**: משתנה שלפיו יש לשמור על יחס זהה של קטגוריות בשני המערכים

### דוגמה עם מערך נתונים אמיתי

```python
import pandas as pd
import numpy as np
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_boston

# Load dataset
boston = load_boston()
X = boston.data
y = boston.target

# Create a dataframe with feature names
boston_df = pd.DataFrame(X, columns=boston.feature_names)
boston_df['PRICE'] = y

# Display first few rows and basic statistics
print(boston_df.head())
print("\nBasic statistics:")
print(boston_df.describe())

# Split the data with stratification
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42)

print(f"\nTraining set: {X_train.shape[0]} samples")
print(f"Testing set: {X_test.shape[0]} samples")
```

## 3. בניית מודל רגרסיה לינארית עם scikit-learn

### יצירת והתאמת מודל רגרסיה לינארית

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score

# Create synthetic data
np.random.seed(42)
X = np.random.rand(100, 1) * 10
y = 2 * X.squeeze() + 1 + np.random.randn(100) * 2  # y = 2x + 1 + noise

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Create and train the model
model = LinearRegression()
model.fit(X_train, y_train)

# Get model parameters
slope = model.coef_[0]
intercept = model.intercept_
print(f"Model equation: y = {slope:.2f}x + {intercept:.2f}")

# Make predictions
y_train_pred = model.predict(X_train)
y_test_pred = model.predict(X_test)

# Calculate metrics
train_mse = mean_squared_error(y_train, y_train_pred)
test_mse = mean_squared_error(y_test, y_test_pred)
train_r2 = r2_score(y_train, y_train_pred)
test_r2 = r2_score(y_test, y_test_pred)

print(f"Training MSE: {train_mse:.2f}")
print(f"Testing MSE: {test_mse:.2f}")
print(f"Training R²: {train_r2:.2f}")
print(f"Testing R²: {test_r2:.2f}")
```

### ויזואליזציה של המודל והתוצאות

```python
# Create subplots
fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Plot training data and predictions
axes[0].scatter(X_train, y_train, color='blue', alpha=0.7, label='Training data')
axes[0].plot(X_train, y_train_pred, color='red', linewidth=2, label='Model predictions')
axes[0].set_title(f'Training Data (R² = {train_r2:.2f})')
axes[0].set_xlabel('X')
axes[0].set_ylabel('y')
axes[0].legend()
axes[0].grid(True, alpha=0.3)

# Plot testing data and predictions
axes[1].scatter(X_test, y_test, color='green', alpha=0.7, label='Testing data')
axes[1].plot(X_test, y_test_pred, color='red', linewidth=2, label='Model predictions')
axes[1].set_title(f'Testing Data (R² = {test_r2:.2f})')
axes[1].set_xlabel('X')
axes[1].set_ylabel('y')
axes[1].legend()
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()

# Plot residuals
fig, axes = plt.subplots(1, 2, figsize=(14, 6))

# Training residuals
train_residuals = y_train - y_train_pred
axes[0].scatter(y_train_pred, train_residuals, alpha=0.7)
axes[0].axhline(y=0, color='red', linestyle='-', linewidth=1)
axes[0].set_title('Training Residuals vs Predictions')
axes[0].set_xlabel('Predicted values')
axes[0].set_ylabel('Residuals')
axes[0].grid(True, alpha=0.3)

# Testing residuals
test_residuals = y_test - y_test_pred
axes[1].scatter(y_test_pred, test_residuals, alpha=0.7, color='green')
axes[1].axhline(y=0, color='red', linestyle='-', linewidth=1)
axes[1].set_title('Testing Residuals vs Predictions')
axes[1].set_xlabel('Predicted values')
axes[1].set_ylabel('Residuals')
axes[1].grid(True, alpha=0.3)

plt.tight_layout()
plt.show()
```

### דוגמה מלאה עם מערך נתונים אמיתי

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from sklearn.datasets import fetch_california_housing
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_squared_error, r2_score
from sklearn.preprocessing import StandardScaler

# Load the California housing dataset
housing = fetch_california_housing()
X = housing.data
y = housing.target

# Create a DataFrame
housing_df = pd.DataFrame(X, columns=housing.feature_names)
housing_df['Price'] = y

# Analysis with pairplot (using a sample to keep it fast)
sample_indices = np.random.choice(len(housing_df), size=1000, replace=False)
housing_sample = housing_df.iloc[sample_indices]

print("Creating pairplot to examine relationships between features...")
sns.pairplot(housing_sample[['MedInc', 'HouseAge', 'AveRooms', 'Price']])
plt.suptitle('Feature Relationships in California Housing Dataset', y=1.02)
plt.show()

# Create correlation matrix
correlation_matrix = housing_df.corr()
plt.figure(figsize=(10, 8))
sns.heatmap(correlation_matrix, annot=True, cmap='coolwarm', linewidths=0.5)
plt.title('Correlation Matrix of Housing Features')
plt.show()

# Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Scale the features
scaler = StandardScaler()
X_train_scaled = scaler.fit_transform(X_train)
X_test_scaled = scaler.transform(X_test)

# Train a linear regression model
model = LinearRegression()
model.fit(X_train_scaled, y_train)

# Print the coefficients
coefficients = pd.DataFrame({
    'Feature': housing.feature_names,
    'Coefficient': model.coef_
})
print("Model Coefficients:")
print(coefficients.sort_values(by='Coefficient', ascending=False))

# Make predictions
y_train_pred = model.predict(X_train_scaled)
y_test_pred = model.predict(X_test_scaled)

# Calculate metrics
train_mse = mean_squared_error(y_train, y_train_pred)
test_mse = mean_squared_error(y_test, y_test_pred)
train_r2 = r2_score(y_train, y_train_pred)
test_r2 = r2_score(y_test, y_test_pred)

print(f"\nTraining MSE: {train_mse:.4f}")
print(f"Testing MSE: {test_mse:.4f}")
print(f"Training R²: {train_r2:.4f}")
print(f"Testing R²: {test_r2:.4f}")

# Plot predictions vs actual values
plt.figure(figsize=(10, 6))
plt.scatter(y_test, y_test_pred, alpha=0.5)
plt.plot([y.min(), y.max()], [y.min(), y.max()], 'r--')
plt.xlabel('Actual Values')
plt.ylabel('Predicted Values')
plt.title('Predicted vs Actual Housing Prices')
plt.grid(True, alpha=0.3)
plt.show()
```

## סיכום

שילוב של pairplot לבדיקת הקשרים בין המשתנים, train_test_split לפיצול נתונים ושימוש במודל רגרסיה לינארית מאפשר לנו לבצע תהליך מלא של ניתוח וחיזוי:

1. **ניתוח חזותי** באמצעות pairplot מאפשר הבנה טובה יותר של הקשרים בין המשתנים ומסייע בבחירת המאפיינים הרלוונטיים למודל.

2. **פיצול נתונים נכון** באמצעות train_test_split מבטיח שנוכל להעריך את המודל באופן אמין.

3. **בניית מודל רגרסיה לינארית** מאפשרת לנו לחזות ערכים ולהבין את ההשפעה של כל מאפיין על התוצאה.

שלושת הכלים הללו יחד מהווים בסיס חשוב בתהליך העבודה עם נתונים, וישיגו תוצאות טובות יותר מאשר שימוש בכל אחד מהם בנפרד.
