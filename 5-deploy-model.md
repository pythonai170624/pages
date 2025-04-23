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

## what about scaling ?

### 1. in Pipeline its automatially

```python
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline
import joblib

# 1. Generate sample data
X = np.array([[1, 5], [2, 6], [3, 7], [4, 8], [5, 9]])
y = np.array([10, 20, 30, 40, 50])

# 2. Create pipeline with scaler and model
model = Pipeline([
    ('scaler', StandardScaler()),
    ('regressor', LinearRegression())
])

# 3. Train the model
model.fit(X, y)
print("Model trained")

# 4. Save the model (with scaler)
model_filename = 'linear_model_with_scaler.joblib'
joblib.dump(model, model_filename)
print(f"Model saved to {model_filename}")

# 5. Load the model
loaded_model = joblib.load(model_filename)
print(f"Model loaded from {model_filename}")

# 6. Make predictions with loaded model
X_new = np.array([[6, 10], [7, 11]])

# AUTOMATIC SCALING (happens inside the pipeline)
predictions = loaded_model.predict(X_new)
print(f"Predictions with automatic scaling: {predictions}")
```

### 2. manually

```python
import numpy as np
from sklearn.linear_model import LinearRegression
from sklearn.preprocessing import StandardScaler
import joblib

# 1. Generate sample data
X = np.array([[1, 5], [2, 6], [3, 7], [4, 8], [5, 9]])
y = np.array([10, 20, 30, 40, 50])

# 2. Create and fit the scaler
scaler = StandardScaler()
X_scaled = scaler.fit_transform(X)
print("Data scaled")

# 3. Create and fit the model (on scaled data)
model = LinearRegression()
model.fit(X_scaled, y)
print("Model trained on scaled data")

# 4. Save both the scaler and model separately
scaler_filename = 'scaler.joblib'
model_filename = 'linear_model.joblib'
joblib.dump(scaler, scaler_filename)
joblib.dump(model, model_filename)
print(f"Scaler saved to {scaler_filename}")
print(f"Model saved to {model_filename}")

# 5. Load the scaler and model
loaded_scaler = joblib.load(scaler_filename)
loaded_model = joblib.load(model_filename)
print(f"Scaler and model loaded from disk")

# 6. Make predictions with loaded model (with explicit scaling)
X_new = np.array([[6, 10], [7, 11]])
# Scale the new data using the loaded scaler
X_new_scaled = loaded_scaler.transform(X_new)
# Make predictions using the loaded model
predictions = loaded_model.predict(X_new_scaled)

print(f"Original new data: \n{X_new}")
print(f"Scaled new data: \n{X_new_scaled}")
print(f"Predictions: {predictions}")

# 7. Verify it works correctly by comparing with original scaler/model
original_X_new_scaled = scaler.transform(X_new)
original_predictions = model.predict(original_X_new_scaled)
print(f"Do the predictions match? {np.allclose(predictions, original_predictions)}")
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
