# מדדי שגיאה ברגרסיה: MAE, MSE, RMSE

## הקדמה

כאשר בונים מודל רגרסיה (חיזוי ערכים רציפים), אנו רוצים להעריך את ביצועי המודל. מדדי שגיאה מאפשרים לנו להבין עד כמה הערכים שהמודל מנבא קרובים לערכים האמיתיים. שלושת המדדים הנפוצים ביותר הם MAE, MSE ו-RMSE.

## MAE - Mean Absolute Error (שגיאה מוחלטת ממוצעת)

### הנוסחה
$$\text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |y_i - \hat{y}_i|$$

כאשר:
- $n$ הוא מספר הדגימות
- $y_i$ הוא הערך האמיתי של הדגימה ה-$i$
- $\hat{y}_i$ הוא הערך שהמודל חזה עבור הדגימה ה-$i$

### הסבר
MAE מחשב את הממוצע של ערכי השגיאה המוחלטים. כלומר, מחשבים את ההפרש המוחלט בין הערך האמיתי לערך החזוי עבור כל דגימה, ואז מחשבים את הממוצע של כל ההפרשים האלה.

### יתרונות
- קל להבנה - התוצאה היא באותן יחידות כמו המשתנה המקורי
- עמיד יותר לחריגים (outliers) מאשר MSE
- אינטואיטיבי - מייצג את השגיאה הממוצעת בערך מוחלט

### חסרונות
- אינו מעניש שגיאות גדולות באופן פרופורציונלי יותר משגיאות קטנות
- לא ניתן לגזירה בנקודת המינימום (בגלל הערך המוחלט), מה שעלול להקשות על אלגוריתמים מסוימים של אופטימיזציה

## MSE - Mean Squared Error (שגיאה ריבועית ממוצעת)

### הנוסחה
$$\text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2$$

### הסבר
MSE מחשב את ממוצע ריבועי השגיאות. מרביעים את ההפרש בין הערך האמיתי לערך החזוי עבור כל דגימה, ואז מחשבים את הממוצע.

### יתרונות
- מעניש שגיאות גדולות יותר באופן ריבועי, מה שמתאים לבעיות רבות
- תמיד ניתן לגזירה, לכן שימושי באלגוריתמים של אופטימיזציה
- נפוץ מאוד בתחום הסטטיסטיקה ולמידת מכונה

### חסרונות
- התוצאה היא ביחידות ריבועיות של המשתנה המקורי (למשל, אם חוזים מחירים בש"ח, ה-MSE יהיה בש"ח בריבוע)
- רגיש מאוד לחריגים בגלל הריבוע
- פחות אינטואיטיבי להבנה בהשוואה ל-MAE

## RMSE - Root Mean Squared Error (שורש השגיאה הריבועית הממוצעת)

### הנוסחה
$$\text{RMSE} = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (y_i - \hat{y}_i)^2} = \sqrt{\text{MSE}}$$

### הסבר
RMSE הוא השורש הריבועי של MSE. זהו ניסיון לשמר את היתרונות של MSE תוך החזרת התוצאה ליחידות המקוריות.

### יתרונות
- התוצאה היא באותן יחידות כמו המשתנה המקורי
- עדיין מעניש שגיאות גדולות יותר באופן לא לינארי
- אינטואיטיבי יותר מ-MSE, אך עדיין שומר על התכונות הסטטיסטיות שלו

### חסרונות
- רגיש לחריגים, כמו MSE
- חישוב מורכב יותר מ-MAE

## השוואה בין המדדים

| מדד | נוסחה | יחידות | רגישות לחריגים | קלות הבנה |
|-----|--------|---------|-----------------|------------|
| MAE | $\frac{1}{n} \sum |y_i - \hat{y}_i|$ | כמו המשתנה המקורי | נמוכה | גבוהה |
| MSE | $\frac{1}{n} \sum (y_i - \hat{y}_i)^2$ | ריבוע המשתנה המקורי | גבוהה | בינונית |
| RMSE | $\sqrt{\frac{1}{n} \sum (y_i - \hat{y}_i)^2}$ | כמו המשתנה המקורי | גבוהה | בינונית |

## דוגמאות מספריות

נניח שיש לנו מודל שחוזה מחירי דירות. להלן מספר דוגמאות אמיתיות וחזויות (במיליוני ש"ח):

| דירה | מחיר אמיתי | מחיר חזוי | שגיאה | שגיאה מוחלטת | שגיאה בריבוע |
|------|------------|------------|--------|--------------|--------------|
| 1    | 1.5        | 1.7        | -0.2   | 0.2          | 0.04         |
| 2    | 2.3        | 2.1        | 0.2    | 0.2          | 0.04         |
| 3    | 3.0        | 2.8        | 0.2    | 0.2          | 0.04         |
| 4    | 1.8        | 1.9        | -0.1   | 0.1          | 0.01         |
| 5    | 4.2        | 3.5        | 0.7    | 0.7          | 0.49         |

### חישוב MAE
$$\text{MAE} = \frac{1}{5} (0.2 + 0.2 + 0.2 + 0.1 + 0.7) = \frac{1.4}{5} = 0.28$$

המשמעות: בממוצע, המודל שלנו טועה ב-0.28 מיליון ש"ח.

### חישוב MSE
$$\text{MSE} = \frac{1}{5} (0.04 + 0.04 + 0.04 + 0.01 + 0.49) = \frac{0.62}{5} = 0.124$$

המשמעות: ממוצע ריבועי השגיאות הוא 0.124 מיליון ש"ח בריבוע.

### חישוב RMSE
$$\text{RMSE} = \sqrt{\text{MSE}} = \sqrt{0.124} \approx 0.352$$

המשמעות: השורש הריבועי של ממוצע ריבועי השגיאות הוא כ-0.35 מיליון ש"ח.

## דוגמה עם חריגים

נוסיף דירה שישית עם טעות חיזוי גדולה:

| דירה | מחיר אמיתי | מחיר חזוי | שגיאה | שגיאה מוחלטת | שגיאה בריבוע |
|------|------------|------------|--------|--------------|--------------|
| 1    | 1.5        | 1.7        | -0.2   | 0.2          | 0.04         |
| 2    | 2.3        | 2.1        | 0.2    | 0.2          | 0.04         |
| 3    | 3.0        | 2.8        | 0.2    | 0.2          | 0.04         |
| 4    | 1.8        | 1.9        | -0.1   | 0.1          | 0.01         |
| 5    | 4.2        | 3.5        | 0.7    | 0.7          | 0.49         |
| 6    | 5.0        | 3.0        | 2.0    | 2.0          | 4.00         |

### חישוב MAE החדש
$$\text{MAE} = \frac{1}{6} (0.2 + 0.2 + 0.2 + 0.1 + 0.7 + 2.0) = \frac{3.4}{6} \approx 0.567$$

### חישוב MSE החדש
$$\text{MSE} = \frac{1}{6} (0.04 + 0.04 + 0.04 + 0.01 + 0.49 + 4.0) = \frac{4.62}{6} = 0.77$$

### חישוב RMSE החדש
$$\text{RMSE} = \sqrt{\text{MSE}} = \sqrt{0.77} \approx 0.878$$

שימו לב כיצד החריג (דירה 6) השפיע באופן שונה על כל מדד:
- ה-MAE גדל פי 2 בערך (מ-0.28 ל-0.567)
- ה-MSE גדל פי 6.2 בערך (מ-0.124 ל-0.77)
- ה-RMSE גדל פי 2.5 בערך (מ-0.352 ל-0.878)

זה מדגים את הרגישות הגבוהה יותר של MSE ו-RMSE לחריגים.

## יישום בפייתון

להלן קוד פייתון שמחשב את שלושת המדדים:

```python
import numpy as np
from sklearn.metrics import mean_absolute_error, mean_squared_error

# True values and predictions
y_true = np.array([1.5, 2.3, 3.0, 1.8, 4.2, 5.0])
y_pred = np.array([1.7, 2.1, 2.8, 1.9, 3.5, 3.0])

# Calculate MAE
mae = mean_absolute_error(y_true, y_pred)
print(f"MAE: {mae:.3f}")

# Calculate MSE
mse = mean_squared_error(y_true, y_pred)
print(f"MSE: {mse:.3f}")

# Calculate RMSE
rmse = np.sqrt(mse)
print(f"RMSE: {rmse:.3f}")

# Calculate manually for verification
manual_mae = np.mean(np.abs(y_true - y_pred))
manual_mse = np.mean((y_true - y_pred)**2)
manual_rmse = np.sqrt(manual_mse)

print("\nManual calculations:")
print(f"MAE: {manual_mae:.3f}")
print(f"MSE: {manual_mse:.3f}")
print(f"RMSE: {manual_rmse:.3f}")
```

תוצאות הקוד:
```
MAE: 0.567
MSE: 0.770
RMSE: 0.878

Manual calculations:
MAE: 0.567
MSE: 0.770
RMSE: 0.878
```

## ויזואליזציה ברגרסיה לינארית

בואו נדגים את שלושת מדדי השגיאה על נתוני רגרסיה לינארית:

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LinearRegression
from sklearn.metrics import mean_absolute_error, mean_squared_error
from sklearn.model_selection import train_test_split

# Create some example data
np.random.seed(42)
X = 2 * np.random.rand(100, 1)
y = 4 + 3 * X + np.random.randn(100, 1)

# Add a few outliers
X = np.vstack([X, [[1.9]], [[1.95]], [[1.85]]])
y = np.vstack([y, [[10]], [[12]], [[14]]])

# Split the data
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# Train a linear regression model
model = LinearRegression()
model.fit(X_train, y_train)

# Make predictions
y_train_pred = model.predict(X_train)
y_test_pred = model.predict(X_test)

# Calculate metrics
train_mae = mean_absolute_error(y_train, y_train_pred)
test_mae = mean_absolute_error(y_test, y_test_pred)

train_mse = mean_squared_error(y_train, y_train_pred)
test_mse = mean_squared_error(y_test, y_test_pred)

train_rmse = np.sqrt(train_mse)
test_rmse = np.sqrt(test_mse)

# Print results
print(f"Training MAE: {train_mae:.3f}")
print(f"Test MAE: {test_mae:.3f}")
print(f"Training MSE: {train_mse:.3f}")
print(f"Test MSE: {test_mse:.3f}")
print(f"Training RMSE: {train_rmse:.3f}")
print(f"Test RMSE: {test_rmse:.3f}")

# Plot the results
plt.figure(figsize=(12, 8))

# Plot data and regression line
plt.subplot(2, 2, 1)
plt.scatter(X_train, y_train, alpha=0.7, label='Training data')
plt.scatter(X_test, y_test, alpha=0.7, color='green', label='Test data')
plt.plot(X, model.predict(X), color='red', linewidth=2, label='Regression line')
plt.title('Linear Regression with Outliers')
plt.xlabel('X')
plt.ylabel('y')
plt.legend()

# Plot residuals
plt.subplot(2, 2, 2)
plt.scatter(y_train_pred, y_train - y_train_pred, alpha=0.7)
plt.scatter(y_test_pred, y_test - y_test_pred, alpha=0.7, color='green')
plt.axhline(y=0, color='red', linestyle='-', linewidth=1)
plt.title('Residuals')
plt.xlabel('Predicted values')
plt.ylabel('Residuals')

# Plot absolute errors (for MAE)
plt.subplot(2, 2, 3)
plt.scatter(X_train, np.abs(y_train - y_train_pred), alpha=0.7, label='Training |error|')
plt.scatter(X_test, np.abs(y_test - y_test_pred), alpha=0.7, color='green', label='Test |error|')
plt.axhline(y=train_mae, color='blue', linestyle='--', linewidth=1, label=f'Train MAE: {train_mae:.3f}')
plt.axhline(y=test_mae, color='green', linestyle='--', linewidth=1, label=f'Test MAE: {test_mae:.3f}')
plt.title('Absolute Errors (MAE)')
plt.xlabel('X')
plt.ylabel('|Actual - Predicted|')
plt.legend()

# Plot squared errors (for MSE/RMSE)
plt.subplot(2, 2, 4)
plt.scatter(X_train, (y_train - y_train_pred)**2, alpha=0.7, label='Training error²')
plt.scatter(X_test, (y_test - y_test_pred)**2, alpha=0.7, color='green', label='Test error²')
plt.axhline(y=train_mse, color='blue', linestyle='--', linewidth=1, label=f'Train MSE: {train_mse:.3f}')
plt.axhline(y=test_mse, color='green', linestyle='--', linewidth=1, label=f'Test MSE: {test_mse:.3f}')
plt.title('Squared Errors (MSE)')
plt.xlabel('X')
plt.ylabel('(Actual - Predicted)²')
plt.legend()

plt.tight_layout()
plt.show()
```

## סיכום ומתי להשתמש בכל מדד

### MAE
משתמשים ב-MAE כאשר:
- רוצים מדד שקל להסביר לאנשים ללא רקע טכני
- הטעויות הגדולות לא בהכרח גרועות יותר באופן דרמטי מהטעויות הקטנות
- יש חריגים בנתונים ואין רוצים שישפיעו יתר על המידה על הערכת המודל

### MSE
משתמשים ב-MSE כאשר:
- צריכים פונקציית הפסד שניתנת לגזירה לצורך אופטימיזציה
- טעויות גדולות הן בעייתיות מאוד ורוצים "להעניש" אותן יותר  
- הנתונים אינם כוללים חריגים משמעותיים, או שהחריגים חשובים ורוצים להתחשב בהם

### RMSE
משתמשים ב-RMSE כאשר:
- רוצים מדד שמעניש טעויות גדולות אך עדיין מפיק תוצאה ביחידות של המשתנה המקורי
- נדרשת התאמה לשיטות סטטיסטיות אחרות
- רוצים מדד שיהיה קל יותר להבנה מ-MSE אך עדיין ישמור על תכונותיו הסטטיסטיות

לעתים קרובות כדאי לחשב את כל שלושת המדדים כדי לקבל תמונה מלאה של ביצועי המודל, במיוחד אם ייתכן שיש חריגים בנתונים.
