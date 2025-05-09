## תרגיל: שימוש במודל SVM לסיווג פירות 🍌🍎

בתרגיל הבא תתבקשו להשתמש באלגוריתם **SVM** על מנת לזהות לאיזו קטגוריה משתייך כל פרי – **תפוח (Apple)** או **בננה (Banana)**.

המשימה כוללת את השלבים הבאים:
- להשתמש בטבלת הנתונים שלמטה, הכוללת שלושה פיצ'רים עבור כל פריט 🍏
- לבנות מודל **SVM** ולבצע **אימון** (training) על הנתונים הקיימים
- לבדוק את **רמת הדיוק של המודל** על קבוצת בדיקה 🧪

| Feature_A | Feature_B | Feature_C | Label   |
|-----------|-----------|-----------|---------|
| 1.2       | 3.5       | 0.5       | banana |
| 1.0       | 3.7       | 0.6       | banana |
| 1.1       | 3.6       | 0.4       | banana |
| 2.0       | 3.9       | 0.7       | banana |
| 2.1       | 4.0       | 0.8       | banana |
| 2.2       | 4.1       | 0.9       | banana |
| 1.9       | 3.8       | 0.7       | banana |
| 2.3       | 4.2       | 0.8       | banana |
| 1.8       | 3.9       | 0.6       | banana |
| 5.0       | 1.1       | 2.1       | apple  |
| 5.2       | 1.0       | 2.2       | apple  |
| 5.1       | 1.2       | 2.3       | apple  |
| 4.9       | 0.9       | 2.0       | apple  |
| 5.3       | 1.3       | 2.4       | apple  |
| 5.4       | 1.4       | 2.5       | apple  |

- ולבסוף... ✨  
  לנבא לאיזו קבוצה משתייך הפרי הבא:

🔍 נתוני פרי חדש: **Feature_A = 1.9**, **Feature_B = 4.0**, **Feature_C = 0.7**  

לאיזה קבוצה הוא שייך?  **Apple** או **Banana**?

קוד עזר:
```python
import pandas as pd
from sklearn.model_selection import train_test_split, GridSearchCV
from sklearn.svm import SVC
from sklearn.metrics import accuracy_score, classification_report

# Step 1: Create the dataset with features and label
data = pd.DataFrame({
    "Feature_A": [1.2, 1.0, 1.1, 2.0, 2.1, 2.2, 1.9, 2.3, 1.8,
                  5.0, 5.2, 5.1, 4.9, 5.3, 5.4],
    "Feature_B": [3.5, 3.7, 3.6, 3.9, 4.0, 4.1, 3.8, 4.2, 3.9,
                  1.1, 1.0, 1.2, 0.9, 1.3, 1.4],
    "Feature_C": [0.5, 0.6, 0.4, 0.7, 0.8, 0.9, 0.7, 0.8, 0.6,
                  2.1, 2.2, 2.3, 2.0, 2.4, 2.5],
    "Label": [
        'banana', 'banana', 'banana', 'banana', 'banana',
        'banana', 'banana', 'banana', 'banana',
        'apple', 'apple', 'apple', 'apple', 'apple', 'apple'
    ]
})

# Step 2: Convert string labels into 0, 1
# drop_first=True can remove 'banana' and keep only 'Label_apple'
#    which will be T F, T is apple and F is banana
# in this example we will not remvoe the column...
data_encoded = pd.get_dummies(data, columns=['Label'], drop_first=False)

print(data_encoded)

# Choose 'Label_apple' explicitly
y = data_encoded['Label_apple'].astype(int)
X = data_encoded.drop(['Label_apple', 'Label_banana'], axis=1)

print(y)

```

## GridSearch

```python
param_grid = {
    'kernel': ['linear', 'rbf', 'poly', 'sigmoid'],
    'C': [0.1, 1, 10, 100]
}
```

כעת השתמש באוסף הפרמטרים כדי למצוא את המודל המתאים ביותר
הדפס מה הפרמטרים האידיאליים שנמצאו