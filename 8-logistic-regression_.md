# רגרסיה לוגיסטית
  
## הבעיה שרוצים לפתור
  
רגרסיה לוגיסטית היא שיטה סטטיסטית המשמשת לניבוי תוצאות בינאריות (0 או 1, כן או לא) על בסיס משתנים מסבירים. בניגוד לרגרסיה לינארית שמנבאת ערכים רציפים, רגרסיה לוגיסטית מחשבת את ההסתברות שדוגמה מסוימת תשתייך לקטגוריה מסוימת.
  
**דוגמה**: נניח שאנו רוצים לחזות האם תלמיד יעבור מבחן (יצליח=1, ייכשל=0) על סמך מספר שעות הלימוד שלו. להלן נתונים היסטוריים של תלמידים:
  
| שעות לימוד | תוצאה (1=עבר, 0=נכשל) |
|------------|----------------------|
| 1          | 0                    |
| 2          | 0                    |
| 3          | 0                    |
| 4          | 0                    |
| 5          | 1                    |
| 6          | 1                    |
| 7          | 1                    |
| 8          | 1                    |
| 9          | 1                    |
  
אנו רוצים למצוא מודל שיחזה את ההסתברות שתלמיד יעבור את המבחן בהתבסס על שעות הלימוד שלו.
  
## Mathematical Formula and Complete Calculation
  
Logistic regression models the probability that a dependent variable Y equals 1 (in our case, passing the exam) as a function of independent variables X (in our case, study hours). The logistic function (also called the sigmoid function) is used to model this probability:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?P(Y=1|X)%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20X)}}"/></p>  
  
  
Where:
- <img src="https://latex.codecogs.com/gif.latex?P(Y=1|X)"/> is the probability that Y=1 given X
- <img src="https://latex.codecogs.com/gif.latex?\beta_0"/> is the intercept
- <img src="https://latex.codecogs.com/gif.latex?\beta_1"/> is the coefficient for the independent variable X
- <img src="https://latex.codecogs.com/gif.latex?e"/> is the base of the natural logarithm (approximately 2.71828)
  
The logistic function always outputs a value between 0 and 1, which can be interpreted as a probability.
  
### 📊 הסבר פשוט על הנוסחה
  
Logistic Regression מחשבת הסתברות שמשהו יקרה, לפי ערך של משתנה X (למשל: כמה שעות למידה)
  
### הנוסחה:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?P(Y%20=%201%20\mid%20X)%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20X)}}"/></p>  
  
  
זו נקראת **פונקציית סיגמואיד (sigmoid function)** – היא מחזירה ערכים בין 0 ל־1, ולכן מתאימה לחיזוי הסתברויות
  
### 🧠 הסברים:
  
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?P(Y%20=%201%20\mid%20X)"/></p>  
 –
  
   ההסתברות ש-Y יהיה 1 בהינתן X
  
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_0"/></p>  
 – הקבוע (intercept)  
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_1"/></p>  
 – מקדם של X  
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?e"/></p>  
 – הבסיס של הלוגריתם הטבעי (בערך 2.718)
  
### 🧮 דוגמה חישובית:
  
אם:
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?X%20=%205"/></p>  
 (שעות למידה)
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_0%20=%20-4"/></p>  
  
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_1%20=%201"/></p>  
  
  
אז:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?P%20=%20\frac{1}{1%20+%20e^{-(-4%20+%201%20\cdot%205)}}%20=%20\frac{1}{1%20+%20e^{-1}}%20\approx%200.73"/></p>  
  
  
כלומר: ההסתברות לעבור את המבחן אחרי 5 שעות למידה היא בערך **73%** ✅
  
### ✨ למה Logistic Regression שימושית?
  
- מחזירה ערכים בין 0 ל־1 → מושלם להסתברויות
- מאפשרת להבין את הקשר בין משתנה רציף (כמו שעות לימוד) לתוצאה בינארית (כמו הצלחה/כישלון)
  
## דוגמא ראשונה: ניחוש β₀ ו־β₁
  
### שלב 1: נגדיר את הבעיה
יש לנו נתונים (X, Y), כאשר:
- X הוא משתנה רציף (למשל: שעות למידה)
- Y הוא בינארי: 0 או 1 (למשל: כישלון או הצלחה)
  
המטרה שלנו:
> לחשב את ההסתברות ש־Y יהיה 1 בהינתן X
  
### שלב 2: נגדיר את הפונקציה הלוגיסטית
  
זו הפונקציה שאיתה נבנה את המודל:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?P(Y%20=%201%20\mid%20X)%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20X)}}"/></p>  
  
  
היא תמיד מחזירה ערכים בין 0 ל־1 – מושלם להסתברויות.
  
### שלב 3: נבנה את פונקציית ההסתברות הכוללת (Likelihood)
  
כדי לבדוק עד כמה המודל שלנו "מדויק", נחשב את ההסתברות שהוא חוזה נכון את כל הנתונים:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?L(\beta_0,%20\beta_1)%20=%20\prod_{i=1}^{n}%20\left(%20\hat{p}_i%20\right)^{y_i}%20\cdot%20\left(1%20-%20\hat{p}_i%20\right)^{1%20-%20y_i}"/></p>  
  
  
כאשר:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_i%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20x_i)}}"/></p>  
  
  
#### ✅ משמעות:
- <img src="https://latex.codecogs.com/gif.latex?\hat{p}_i"/> היא ההסתברות של המודל לכך שהתצפית ה-<img src="https://latex.codecogs.com/gif.latex?i"/> תהיה 1.
- <img src="https://latex.codecogs.com/gif.latex?y_i"/> הוא הערך בפועל — או 0 או 1.
- כאשר <img src="https://latex.codecogs.com/gif.latex?y_i%20=%201"/>, מקבלים: <img src="https://latex.codecogs.com/gif.latex?\hat{p}_i"/>
- כאשר <img src="https://latex.codecogs.com/gif.latex?y_i%20=%200"/>, מקבלים: <img src="https://latex.codecogs.com/gif.latex?1%20-%20\hat{p}_i"/>
  
הנוסחה חכמה בכך שהיא משתמשת ב-<img src="https://latex.codecogs.com/gif.latex?y_i"/> ו-<img src="https://latex.codecogs.com/gif.latex?1%20-%20y_i"/> כדי לכסות את שני המקרים באותה נוסחה — בלי תנאים מיוחדים
  
**🎯 דוגמה לחישוב פונקציית Likelihood**
  
נניח שיש לנו שתי דוגמאות בלבד:
  
- דוגמה ראשונה: x = 1, y = 1. לדוגמא - אחרי שעה של הליכה היה היה עודף סוכר בדם
- דוגמה שנייה: x = 2, y = 0. לדוגמא - אחרי שעתיים של הליכה לא היה עודף סוכר בדם
  
נבחר מקדמים למודל:
- β₀ = 0  
- β₁ = 1
  
**שלב א: נחשב את ההסתברות (p-hat) שהמודל חוזה**
  
הנוסחה היא:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_i%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20x_i)}}"/></p>  
  
  
##### עבור הדוגמה הראשונה (x = 1):
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_1%20=%20\frac{1}{1%20+%20e^{-(0%20+%201%20\cdot%201)}}%20=%20\frac{1}{1%20+%20e^{-1}}%20\approx%200.731"/></p>  
  
  
##### עבור הדוגמה השנייה (x = 2):
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_2%20=%20\frac{1}{1%20+%20e^{-(0%20+%201%20\cdot%202)}}%20=%20\frac{1}{1%20+%20e^{-2}}%20\approx%200.881"/></p>  
  
  
אבל – בגלל ש־y = 0 במקרה הזה, נשתמש ב־(1 − p̂):
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?1%20-%20\hat{p}_2%20\approx%201%20-%200.881%20=%200.119"/></p>  
  
  
  
**שלב ב: נחשב את פונקציית ה־Likelihood הכוללת**
  
נשתמש בנוסחה:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?L%20=%20(\hat{p}_1)^{y_1}%20\cdot%20(1%20-%20\hat{p}_2)^{1%20-%20y_2}"/></p>  
  
  
נציב את הערכים:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?L%20=%200.731%20\cdot%200.119%20\approx%200.087"/></p>  
  
  
  
#### 🧠 מה זה אומר?
  
- המודל חזה טוב יחסית בדוגמה הראשונה (73% עבור y=1)
- אבל טעה בדוגמה השנייה — חזה הסתברות גבוהה ל־y=1, למרות שהתשובה הייתה y=0
- לכן, פונקציית ה־Likelihood הסופית היא בערך 8.7%
  
המסקנה: נרצה למצוא β₀ ו־β₁ אחרים שייתנו ערך גבוה יותר ל־L.
  
  
## כיצד למצוא את β₀ ו־β₁
  
- מחשבים נגזרות של הפונקציה לפי β₀ ו־β₁
- משווים ל־0 כדי למצוא נקודת קיצון
- אבל... אי אפשר לפתור את זה ידנית כמו ברגרסיה רגילה
  
#### מה המטרה?
אנחנו רוצים למצוא את הערכים של לפי β₀ ו־β₁ שמביאים את פונקציית הלוג־לייקלי למקסימום
  
לפי מתמטיקה:
- נקודת מקסימום נמצאת כאשר **הנגזרת = 0**
- כלומר: הפונקציה לא עולה ולא יורדת – זו פסגה (או תחתית, אבל כאן מדובר על מקסימום)
  
#### מהי הפונקציה שלנו?
  
זוהי פונקציית **הלוג־לייקלי** (log-likelihood):
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\log%20L(\beta_0,%20\beta_1)%20=%20\sum_{i=1}^{n}%20\left[%20y_i%20\cdot%20\log(\hat{p}_i)%20+%20(1%20-%20y_i)%20\cdot%20\log(1%20-%20\hat{p}_i)%20\right]"/></p>  
  
  
כאשר:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_i%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20x_i)}}"/></p>  
  
  
#### נגזרות:
  
הנגזרות של הפונקציה לפי β₀ ו־β₁ הן:
  
- לפי <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_1"/></p>  
:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial}{\partial%20\beta_1}%20\log%20L%20=%20\sum_{i=1}^{n}%20(y_i%20-%20\hat{p}_i)%20\cdot%20x_i"/></p>  
  
  
- לפי <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_0"/></p>  
:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial}{\partial%20\beta_0}%20\log%20L%20=%20\sum_{i=1}^{n}%20(y_i%20-%20\hat{p}_i)"/></p>  
  
  
  
#### למה משווים ל־0?
  
כדי למצוא מקסימום, משווים את הנגזרות ל־0:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial}{\partial%20\beta}%20\log%20L%20=%200"/></p>  
  
  
אבל... לא ניתן לפתור ישירות, לכן נעבור לפתרון נומרי (כמו Gradient Descent)
  
## 🧸 דוגמה לתהליך 
  
נניח שיש לנו 2 תצפיות:
  
| x  | y |
|----|---|
| 1  | 1 |
| 2  | 0 |
  
ונניח שכרגע:
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_0%20=%200"/></p>  
  
- <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_1%20=%200"/></p>  
  
  
#### שלב 1: חישוב ההסתברויות (p-hat)
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_1%20=%20\frac{1}{1%20+%20e^{-(0%20+%200%20\cdot%201)}}%20=%200.5"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{p}_2%20=%20\frac{1}{1%20+%20e^{-(0%20+%200%20\cdot%202)}}%20=%200.5"/></p>  
  
  
#### שלב 2: 
  
**חישוב נגזרת לפי <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_1"/></p>  
**
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial}{\partial%20\beta_0}%20\log%20L%20=%20\sum_{i=1}^{n}%20(y_i%20-%20\hat{p}_i)"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\sum%20(y_i%20-%20\hat{p}_i)%20\cdot%20x_i%20=(1%20-%200.5)%20\cdot%201%20+%20(0%20-%200.5)%20\cdot%202%20=0.5%20-%201%20=%20-0.5"/></p>  
  
  
מאחר והנגזרת שלילית, המסקנה היא:
> **צריך להגדיל את** <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_1"/></p>  
 כדי לשפר את המודל.
> 
  
**חישוב נגזרת לפי <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_0"/></p>  
**
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\frac{\partial}{\partial%20\beta_0}%20\log%20L%20=%20\sum_{i=1}^{n}%20(y_i%20-%20\hat{p}_i)"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\sum%20(y_i%20-%20\hat{p}_i)%20=(1%20-%200.5)%20+%20(0%20-%200.5)%20%20=0.5%20-%200.5%20=%200"/></p>  
  
  
- הנגזרת לפי β₀ שווה ל־0 בסיבוב הזה
- לכן **לא נעדכן את β₀** כרגע, כי אין טעם לזוז אם אנחנו בדיוק באמצע
  
### 🎯 סיכום:
- אם הנגזרת חיובית → מעלים/מורידים את המקדם תלוי בפונקציית העלות (חיובית/שלילית)
- אם הנגזרת שלילית → מורידים/מעלים את המקדם תלוי בפונקציית העלות (חיובית/שלילית)
- נמשיך כך עד שהנגזרת תהיה ≈ 0 – ונמצא את מקסימום הפונקציה 💛
  
### משתמשים בפתרון נומרי
  
בגלל שהפונקציה מסובכת, המחשב פותר אותה על ידי ניסוי וטעייה חכמה:
  
- Gradient Descent
- Newton-Raphson
- שיטות אחרות שנמצאות במודלים כמו `LogisticRegression` בסקיקיט־לרן
  
כל פעם המחשב משנה קצת את β₀ ו־β₁, בודק אם ההסתברויות נהיו יותר טובות,  
וממשיך עד שהוא מוצא את הערכים הכי טובים.
  
### ✅ סיכום:
- אין נוסחה סגורה למציאת β₀ ו־β₁
- הפתרון מתקבל בעזרת אופטימיזציה מתמטית (נומרית)
- הפונקציה שאנחנו ממקסמים נקראת log-likelihood
  
---
  
**מציאת המקדמים בפייטון:**
  
```python
from sklearn.linear_model import LogisticRegression
import numpy as np
  
# Data: Annual income and loan repayment (1=yes, 0=no)
X = np.array([30, 35, 40, 45, 50, 55, 60, 65, 70, 75]).reshape(-1, 1)
y = np.array([0, 0, 0, 0, 0, 1, 0, 1, 1, 1])
  
# Fit logistic regression model
model = LogisticRegression(solver='liblinear')
model.fit(X, y)
  
# Get coefficients
b0 = model.intercept_[0]
b1 = model.coef_[0][0]
  
# Print the logistic regression equation
print(f"Logistic regression equation:")
print(f"P(return) = 1 / (1 + e^-({b0:.2f} + {b1:.2f} * income))")
```
output:
```
Logistic regression equation:
P(return) = 1 / (1 + e^-(-0.89 + 0.02 * income))
```
  
  
### Decision Boundary
  
In logistic regression, we often need to make a binary decision based on the predicted probability. The most common threshold is 0.5, meaning:
- If <img src="https://latex.codecogs.com/gif.latex?P(Y=1|X)%20\geq%200.5"/>, we predict Y=1 (pass)
- If <img src="https://latex.codecogs.com/gif.latex?P(Y=1|X)%20&lt;%200.5"/>, we predict Y=0 (fail)
  
Let's find the decision boundary where <img src="https://latex.codecogs.com/gif.latex?P(\text{pass}|\text{hours})%20=%200.5"/>:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?0.5%20=%20\frac{1}{1%20+%20e^{-(-6.7%20+%201.5%20\times%20\text{hours})}}"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?1%20+%20e^{-(-6.7%20+%201.5%20\times%20\text{hours})}%20=%202"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?e^{-(-6.7%20+%201.5%20\times%20\text{hours})}%20=%201"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?-(-6.7%20+%201.5%20\times%20\text{hours})%20=%200"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?-6.7%20+%201.5%20\times%20\text{hours}%20=%200"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?1.5%20\times%20\text{hours}%20=%206.7"/></p>  
  
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\text{hours}%20=%20\frac{6.7}{1.5}%20\approx%204.47"/></p>  
  
  
So according to our model, students need to study approximately 4.47 hours to have a 50% chance of passing the exam.
  
## גרף
  
<img src="log2.png" style="width:80%;"/>
  
הגרף מציג את עקומת הרגרסיה הלוגיסטית (הקו הכחול) שמתאר את ההסתברות לעבור את המבחן כפונקציה של שעות הלימוד. שימו לב לצורה האופיינית של עקומת ה-S (סיגמואיד) של
הרגרסיה הלוגיסטית. הנקודות האדומות הן הנתונים המקוריים, והקו המקווקו האנכי מציין את הגבול שבו ההסתברות לעבור את המבחן היא 0.5.
  
## קוד פייטון
  
הנה קוד פייטון ליישום רגרסיה לוגיסטית:
  
```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.linear_model import LogisticRegression
import pandas as pd
from math import log
  
# Our data
hours_studied = np.array([1, 2, 3, 4, 5, 6, 7, 8, 9]).reshape(-1, 1)  # Study hours
exam_results = np.array([0, 0, 0, 0, 1, 1, 1, 1, 1])  # Exam results (0=fail, 1=pass)
  
# Create logistic regression model
model = LogisticRegression(solver='liblinear')
model.fit(hours_studied, exam_results)
  
# Print results
print(f"Intercept (β₀): {model.intercept_[0]:.2f}")
print(f"Coefficient (β₁): {model.coef_[0][0]:.2f}")
  
# Calculate equation
equation = f"P(pass) = 1 / (1 + e^-({model.intercept_[0]:.2f} + {model.coef_[0][0]:.2f} × hours))"
print(f"Logistic equation: {equation}")
  
# Helper: get boundary for any probability
def get_threshold(probability):
    logit = log(probability / (1 - probability))
    return (logit - b0) / b1
  
# Coefficients
b0 = model.intercept_[0]
b1 = model.coef_[0][0]
  
# Find decision boundary (probability = 0.5)
decision_boundary = -model.intercept_[0] / model.coef_[0][0]
# or
decision_boundary = get_threshold(0.5)
  
print(f"Decision boundary: {decision_boundary:.2f} hours")
  
# Generate points for the logistic curve
x_test = np.linspace(0, 10, 100).reshape(-1, 1)
y_proba = model.predict_proba(x_test)[:, 1]
  
# Create the graph
plt.figure(figsize=(10, 6))
plt.scatter(hours_studied, exam_results, color='red', marker='o', label='Data points')
plt.plot(x_test, y_proba, color='blue', label='Logistic curve')
plt.axvline(x=decision_boundary, color='green', linestyle='--', label=f'Decision boundary ({decision_boundary:.2f} hours)')
  
# Add labels and formatting
plt.title('Logistic Regression - Study Hours vs. Exam Result')
plt.xlabel('Study Hours')
plt.ylabel('Probability of Passing Exam')
plt.grid(True, alpha=0.3)
plt.legend()
plt.text(1, 0.8, equation, fontsize=10, bbox=dict(facecolor='white', alpha=0.5))
plt.ylim(-0.05, 1.05)
plt.show()
  
# Make predictions for specific study hours
print("\nPredictions:")
for hours in [1, 2, 3, 4, 5, 6]:
    probability = 1 / (1 + np.exp(-(model.intercept_[0] + model.coef_[0][0] * hours)))
    outcome = "Pass" if probability >= 0.5 else "Fail"
    print(f"{hours} hours: {probability:.2f} probability of passing ({outcome})")
```
  
## דוגמת הרצה
  
כאשר נריץ את הקוד, נקבל:
  
<img src="log1.png" />
  
```
Intercept (β₀): -1.04
Coefficient (β₁): 0.38
Logistic equation: P(pass) = 1 / (1 + e^-(-1.04 + 0.38 × hours))
Decision boundary: 2.72 hours
  
Predictions:
1 hours: 0.34 probability of passing (Fail)
2 hours: 0.43 probability of passing (Fail)
3 hours: 0.53 probability of passing (Pass)
4 hours: 0.62 probability of passing (Pass)
5 hours: 0.70 probability of passing (Pass)
6 hours: 0.78 probability of passing (Pass)
```
  
התוצאות מראות שתלמיד צריך ללמוד לפחות 4.43 שעות כדי שהסיכוי שלו לעבור את המבחן יהיה מעל 50%. כמו כן, תלמיד שלומד 6 שעות יש לו סיכוי של כ-78% לעבור את המבחן.
  
## השוואה בין רגרסיה לינארית לרגרסיה לוגיסטית
  
| היבט | רגרסיה לינארית | רגרסיה לוגיסטית |
|------|-----------------|------------------|
| **סוג משתנה תלוי** | רציף (מספרי) | בינארי (0/1, כן/לא) |
| **טווח התחזית** | כל ערך ממשי <img src="https://latex.codecogs.com/gif.latex?(-\infty,%20\infty)"/> | ערך בין 0 ל-1 (הסתברות) |
| **פונקציית הקישור** | זהות <img src="https://latex.codecogs.com/gif.latex?(y%20=%20\beta_0%20+%20\beta_1%20x)"/> | לוגיט <img src="https://latex.codecogs.com/gif.latex?(\ln\frac{p}{1-p}%20=%20\beta_0%20+%20\beta_1%20x)"/> |
| **שיטת אמידה** | שיטת הריבועים הפחותים | שיטת הנראות המקסימלית |
| **פרשנות המקדמים** | שינוי ב-Y ליחידה של X | שינוי בלוג של יחס הסיכויים |
| **צורת הקשר** | קווי | עקומת S (סיגמואיד) |
| **הנחות** | ליניאריות, נורמליות של שאריות | לא מניחה התפלגות נורמלית |
  
## יישומים של רגרסיה לוגיסטית
  
רגרסיה לוגיסטית משמשת במגוון תחומים:
  
1. **רפואה**: חיזוי הסיכוי להתפתחות מחלה בהתבסס על גורמי סיכון
2. **פיננסים**: הערכת סיכון אשראי וחיזוי האם לקוח יחזיר הלוואה
3. **שיווק**: חיזוי האם לקוח ירכוש מוצר מסוים
4. **חינוך**: חיזוי הצלחה או כישלון של תלמידים
5. **מדעי החברה**: ניתוח של החלטות בינאריות
  
## תרגיל נוסף
  
**תרגיל**: 
חברת אשראי רוצה לחזות האם לקוח יחזיר את ההלוואה שלו (1=החזיר, 0=לא החזיר) על סמך ההכנסה השנתית שלו (באלפי שקלים). להלן נתונים היסטוריים:
  
| הכנסה שנתית (אלפי ש"ח) | החזיר הלוואה (1=כן, 0=לא) |
|------------------------|---------------------------|
| 30                     | 0                         |
| 35                     | 0                         |
| 40                     | 0                         |
| 45                     | 0                         |
| 50                     | 0                         |
| 55                     | 1                         |
| 60                     | 0                         |
| 65                     | 1                         |
| 70                     | 1                         |
| 75                     | 1                         |
| 80                     | 1                         |
| 85                     | 1                         |
| 90                     | 1                         |
  
1. בנה מודל רגרסיה לוגיסטית שמתאר את הקשר בין ההכנסה השנתית לבין החזר ההלוואה.
2. מה ההסתברות שלקוח עם הכנסה שנתית של 58 אלף ש"ח יחזיר את ההלוואה?
3. מהי ההכנסה השנתית המינימלית שלקוח צריך כדי שההסתברות שיחזיר את ההלוואה תהיה לפחות 75%?
  