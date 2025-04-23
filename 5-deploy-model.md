# Saving and Loading Linear Regression Models with Joblib

## הקדמה

מדריך זה מציג כיצד לאמן, לשמור ולטעון מודל רגרסיה לינארית באמצעות scikit-learn ו-joblib. Joblib מספק כלים יעילים לסריאליזציה ודה-סריאליזציה של אובייקטים בפייתון, מה שהופך אותו לאידיאלי עבור פריסת מודלים של למידת מכונה בסביבות ייצור.

## Training a Linear Regression Model

First, let's create and train a simple linear regression model:

```python
import numpy as np
import pandas as pd
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
import joblib  # required
import matplotlib.pyplot as plt

# Create sample data
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

# Split data into training and testing sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Create and train the model
model = LinearRegression()
model.fit(X_train, y_train)

# Print model coefficients
print(f"Intercept: {model.intercept_[0]:.4f}")
print(f"Coefficient: {model.coef_[0][0]:.4f}")
```

Output:
```
Intercept: 3.9272
Coefficient: 2.9857
```

## Saving the Model with Joblib

After training, save the model to disk:

```python
# Save the model to disk
joblib.dump(model, 'linear_regression_model.joblib')
print("Model saved successfully!")
```

Output:
```
Model saved successfully!
```

## Loading the Model with Joblib

Later, you can load the model from disk:

```python
# Load the model from disk
loaded_model = joblib.load('linear_regression_model.joblib')
print("Model loaded successfully!")

# Verify the loaded model has the same coefficients
print(f"Loaded Model Intercept: {loaded_model.intercept_[0]:.4f}")
print(f"Loaded Model Coefficient: {loaded_model.coef_[0][0]:.4f}")
```

Output:
```
Model loaded successfully!
Loaded Model Intercept: 3.9272
Loaded Model Coefficient: 2.9857
```

## Using the Loaded Model for Predictions

Now we can use the loaded model to make predictions:

```python
# Make predictions using the loaded model
y_pred = loaded_model.predict(X_test)

# Evaluate the model
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print(f"Mean Squared Error: {mse:.4f}")
print(f"R² Score: {r2:.4f}")
```

Output:
```
Mean Squared Error: 0.8603
R² Score: 0.8926
```

## Complete Example

Here's a complete example with error handling:

```python
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import mean_squared_error, r2_score
import joblib

# Create sample data
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

# Split data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Train model
print("Training linear regression model...")
model = LinearRegression()
model.fit(X, y)
print(f"Model trained. Intercept: {model.intercept_[0]:.4f}, Coefficient: {model.coef_[0][0]:.4f}")

# Save model
joblib.dump(model, "linear_regression_model.joblib")

# Load model
loaded_model = joblib.load("linear_regression_model.joblib")

if (loaded_model.intercept_[0] == model.intercept_[0] and
    loaded_model.coef_[0][0] == model.coef_[0][0]):
    print("Verification successful: Original and loaded models have identical coefficients")
else:
    print("Verification failed: Models have different coefficients")

# Make predictions
y_pred = loaded_model.predict(X_test)

# Evaluate
mse = mean_squared_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

print(f"Model Evaluation Results:")
print(f"Mean Squared Error: {mse:.4f}")
print(f"R² Score: {r2:.4f}")
```

## Saving with Compression

For larger models, you might want to use compression:

```python
# Save with compression
joblib.dump(model, 'linear_regression_model_compressed.joblib.gz', compress=('gzip', 3))
print("Compressed model saved successfully!")
```
