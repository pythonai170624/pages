# Evaluation של עצי החלטה

**Evaluation** (הערכה) של עץ החלטה עוזרת לנו להבין **כמה טוב המודל שלנו עובד**  
המטרה היא לבדוק איך העץ מתפקד על **נתונים שלא ראה לפני כן** (נתוני בדיקה), ולוודא שהוא **לא עושה אוברפיטינג** או **תת-התאמה**

---

## מטריקות להערכת עצי החלטה

### עבור סיווג (Classification):

1. **Accuracy (דיוק)**  
   - אחוז התחזיות הנכונות מתוך כלל התחזיות  
   - מתאים כשיש **איזון** בין הקטגוריות

2. **Precision (דיוק חיובי)**  
   - כמה מתוך התחזיות שהיו **חיוביות** היו נכונות  
   - חשוב כשעלות של **False Positives** גבוהה

3. **Recall (רגישות)**  
   - כמה מתוך כל המקרים **החיוביים האמיתיים** זוהו נכון  
   - חשוב כשעלות של **False Negatives** גבוהה

4. **F1 Score**  
   - ממוצע הרמוני של **Precision** ו-**Recall**  
   - טוב כשיש **אי-איזון** בין הקטגוריות

5. **Confusion Matrix**  
   - טבלה שמציגה את מספר התחזיות בכל קטגוריה: True Positives, False Positives, True Negatives, False Negatives

#### דוגמה בקוד 

```python
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
from sklearn.model_selection import train_test_split

# Example data: [feature], target (simple regression)
X = [[1], [2], [3], [4], [5], [6], [7], [8], [9], [10]]
y = [1.5, 3.7, 2.5, 4.1, 5.0, 5.9, 7.2, 8.1, 9.0, 10.5]

# Split data into Train and Test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Create and train the Decision Tree Regressor
model = DecisionTreeRegressor(max_depth=3)
model.fit(X_train, y_train)

# Make predictions on the test set
y_pred = model.predict(X_test)

# Evaluate the model
mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

# Predict a single new value (e.g., for feature 5.5)
new_value = [[5.5]]
single_pred = model.predict(new_value)

print(f'Prediction for input {new_value[0][0]}: {single_pred[0]}')

# Print evaluation metrics
print(f'\nMean Squared Error (MSE): {mse}')
print(f'Mean Absolute Error (MAE): {mae}')
print(f'R² Score: {r2}')
```

Output:

```
Prediction for input 5.5: 5.0

Mean Squared Error (MSE): 2.1533333333333338
Mean Absolute Error (MAE): 1.3333333333333337
R² Score: 0.5444287729196049
```

---

### עבור רגרסיה (Regression):

1. **Mean Squared Error (MSE)**  
   - ממוצע ריבועי השגיאות

2. **Mean Absolute Error (MAE)**  
   - ממוצע ערכי השגיאות המוחלטים

3. **R² Score (מקדם הדטרמינציה)**  
   - מודד כמה טוב המודל מסביר את השונות בנתונים

#### דוגמה בקוד 

```python
from sklearn.tree import DecisionTreeRegressor
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
from sklearn.model_selection import train_test_split

# Example data: [feature], target (simple regression)
X = [[1], [2], [3], [4], [5], [6], [7], [8], [9], [10]]
y = [1.5, 3.7, 2.5, 4.1, 5.0, 5.9, 7.2, 8.1, 9.0, 10.5]

# Split data into Train and Test sets
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Create and train the Decision Tree Regressor
model = DecisionTreeRegressor(max_depth=3)
model.fit(X_train, y_train)

# Make predictions on the test set
y_pred = model.predict(X_test)

# Evaluate the model
mse = mean_squared_error(y_test, y_pred)
mae = mean_absolute_error(y_test, y_pred)
r2 = r2_score(y_test, y_pred)

# Predict a single new value (e.g., for feature 5.5)
new_value = [[5.5]]
single_pred = model.predict(new_value)

print(f'Prediction for input {new_value[0][0]}: {single_pred[0]}')

# Print evaluation metrics
print(f'\nMean Squared Error (MSE): {mse}')
print(f'Mean Absolute Error (MAE): {mae}')
print(f'R² Score: {r2}')
```

Output:

```
Prediction for input 5.5: 5.0

Mean Squared Error (MSE): 2.1533333333333338
Mean Absolute Error (MAE): 1.3333333333333337
R² Score: 0.5444287729196049
```


