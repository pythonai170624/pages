# Homework Assignment

## Question 1:  

### Tasks:
1. Calculate the **Mean Absolute Error (MAE)**.  
2. Calculate the **Mean Squared Error (MSE)**.  
3. Calculate the **Root Mean Squared Error (RMSE)**.

<a href="https://github.com/pythonai170624/pages/blob/main/5-deploy-model.md">see example here</a>

```python
import numpy as np
from sklearn.metrics import mean_absolute_error, mean_squared_error

y_true = np.array([
    10.5, 8.2, 7.3, 15.4, 12.0,
    9.8, 6.7, 11.2, 13.5, 9.0,
    8.1, 14.2, 10.0, 7.5, 12.8,
    9.3, 11.8, 8.9, 10.7, 13.1
])

# predicted values
y_pred = np.array([
    11.2, 7.8, 7.0, 16.1, 11.5,
    9.5, 7.2, 10.8, 14.0, 8.7,
    8.5, 13.9, 10.4, 7.8, 12.5,
    9.0, 12.3, 9.4, 11.1, 12.8
])

# calculate here

```

## Question 2:

### Tasks:
1. use the code below
2. save the model
3. in a new program, load the model and ask the user to input a number, show the prediction for this value

```python
import numpy as np
from sklearn.linear_model import LinearRegression

# Create simple data with 10 data points
X = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9, 10]).reshape(-1, 1)  # reshape for sklearn
y = np.array([2, 4, 5, 4, 5, 7, 8, 9, 11, 12])

# Fit the model
model = LinearRegression()
model.fit(X, y)
print("\nModel fitted")
print(f"Coefficient: {model.coef_[0]:.4f}")
print(f"Intercept: {model.intercept_:.4f}")

new_data = np.array([5]).reshape(-1, 1)
new_predictions = model.predict(new_data)
print("Predictions on new data:", new_predictions)
```

dump the model then load the model 