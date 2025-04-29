# 🌟 רגרסיה לוגיסטית מולטיוואריאטית לפלט בינארי (Binary Logistic Regression)
  
## 📘 מה זו רגרסיה לוגיסטית בינארית?
רגרסיה לוגיסטית בינארית היא שיטה סטטיסטית המיועדת לחיזוי משתנה תלוי שיש לו בדיוק **שתי אפשרויות בלבד** (למשל: הצלחה/כישלון, כן/לא, חיובי/שלילי), תוך שימוש בכמה משתנים מסבירים (features).
  
---
  
## ✏️ מושגים בסיסיים:
- **משתנה תלוי (Y)**: משתנה בינארי שאנו מנסים לחזות (למשל: עבר/נכשל במבחן).
- **משתנים בלתי תלויים (X₁, X₂, X₃, ...)**: התכונות המסבירות את Y.
- **β (בטא)**: מקדמים שנלמדים על ידי המודל. כל משתנה מקבל β משלו.
  
---
  
## 📐 נוסחה מתמטית:
ההסתברות שהמשתנה Y יהיה 1, בהתחשב ב־n משתנים מסבירים:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?P(y%20=%201%20\mid%20x)%20=%20\frac{1}{1%20+%20e^{-(\beta_0%20+%20\beta_1%20x_1%20+%20\beta_2%20x_2%20+%20\dots%20+%20\beta_n%20x_n)}}"/></p>  
  
  
- <img src="https://latex.codecogs.com/gif.latex?\beta_0"/>: intercept (היסט)
- <img src="https://latex.codecogs.com/gif.latex?\beta_1,%20\dots,%20\beta_n"/>: מקדמים של התכונות
- <img src="https://latex.codecogs.com/gif.latex?x_1,%20\dots,%20x_n"/>: ערכי התכונות
- <img src="https://latex.codecogs.com/gif.latex?e"/>: בסיס הלוגריתם הטבעי (≈ 2.718)
  
הנוסחה מחזירה הסתברות בין 0 ל־1.
  
---
  
## 💬 דוגמה מילולית:
  
נניח שאנו רוצים לחזות אם סטודנט יעבור מבחן (כן/לא) לפי שעות למידה (X₁) ומספר תרגילים שנפתרו (X₂):
  
למשל:
  
- סטודנט שלמד 10 שעות ופתר 15 תרגילים
- נניח שהמודל חישב את הציון (logit):
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?z%20=%20\beta_0%20+%20\beta_1%20\cdot%2010%20+%20\beta_2%20\cdot%2015"/></p>  
  
  
נניח:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?z%20=%20-1%20+%200.2%20\cdot%2010%20+%200.1%20\cdot%2015%20=%201.5"/></p>  
  
  
ההסתברות שהסטודנט יעבור:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?P(עבר)%20=%20\frac{1}{1%20+%20e^{-1.5}}%20\approx%200.8176"/></p>  
  
  
כלומר, הסתברות של כ־81.8% שהסטודנט יעבור.
  
---
  
## 🔍 פירוש המקדמים (β):
- מקדם חיובי (β>0) מציין שככל שהתכונה גדלה, עולה ההסתברות לתוצאה חיובית (y=1).
- מקדם שלילי (β<0) מציין שככל שהתכונה גדלה, ההסתברות לתוצאה חיובית (y=1) יורדת.
- מקדם קרוב ל־0 מציין שהתכונה אינה משפיעה משמעותית על התוצאה.
  
---
  
## 🧪 דוגמת קוד בפייתון:
```python
import pandas as pd
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.metrics import classification_report, confusion_matrix
  
# טעינת נתונים
df = pd.read_csv('students.csv')
X = df[['hours_studied', 'exercises_done']]
y = df['passed_exam']  # 0 או 1
  
# פיצול הנתונים
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
  
# אימון המודל
model = LogisticRegression()
model.fit(X_train, y_train)
  
# ניבוי ובדיקת המודל
y_pred = model.predict(X_test)
print(classification_report(y_test, y_pred))
print(confusion_matrix(y_test, y_pred))
```
  
---
  
## 🧮 כיצד לפרש את המטריצת בלבול (Confusion Matrix):
- **True Positive (TP)**: חזה "1" והערך האמיתי הוא "1".
- **True Negative (TN)**: חזה "0" והערך האמיתי הוא "0".
- **False Positive (FP)**: חזה "1" אך הערך האמיתי הוא "0".
- **False Negative (FN)**: חזה "0" אך הערך האמיתי הוא "1".
  
---
  
## 🎯 מטרה מעשית של המודל:
לזהות במדויק ככל האפשר את הקטגוריה הנכונה מבין השתיים על סמך המשתנים המסבירים.
  
---
  
הסבר זה מותאם במיוחד לרגרסיה לוגיסטית עם משתנים מרובים ופלט בינארי.
  
  