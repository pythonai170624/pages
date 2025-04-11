# 🧠 השוואה בין פונקציות רגרסיה בפייתון (Linear / Polynomial)

מטרת המסמך: להבין מתי להשתמש באיזו פונקציית רגרסיה בפייתון עבור מודלים לינאריים ופולינומיאליים.

---

## 📘 1. `sklearn.linear_model.LinearRegression`

```python
from sklearn.linear_model import LinearRegression
```

- ✅ מתאימה לרגרסיה לינארית (פיצ'ר אחד או יותר)
- ✅ תומכת בהכשרה נוחה עם `.fit(X, y)`
- ✅ מספקת `.predict(X)`, וגם `.coef_`, `.intercept_`
- ❌ לא תומכת ישירות ב־p-values או סטטיסטיקה מתקדמת

💡 **מעולה אם אתה רוצה מודל מהיר לעבודה פרקטית ולשימוש בפייפליין.**

---

## 📘 2. `sklearn.preprocessing.PolynomialFeatures` + `LinearRegression`

```python
from sklearn.preprocessing import PolynomialFeatures
```

- ✅ מאפשר הרחבת X ל־x, x², x³ וכן הלאה
- ✅ תומך ב־multivariate + interactions
- ❌ דורש עבודה עם `Pipeline` או `fit_transform` על X
- ❌ לא נותן סטטיסטיקה כמו R² Adjusted או p-values

💡 **טוב מאוד כשאתה רוצה פולינום מסודר, עם תמיכה ב־train_test_split ושאר כלים.**

---

## 📘 3. `numpy.polyfit` / `numpy.poly1d`

```python
import numpy as np
np.polyfit(x, y, deg=3)
np.poly1d(...)
```

- ✅ הכי קל ופשוט לשימוש עם וקטורים
- ✅ מחזיר מקדמים של הפולינום
- ❌ לא תומך ביותר מפיצ’ר אחד
- ❌ לא נותן R², סטטיסטיקה או תכונות מתקדמות

💡 **מושלם לגרפים פשוטים ומהירים – כמו גרף נתוני עקומה.**

---

## 📘 4. `statsmodels.api.OLS`

```python
import statsmodels.api as sm
model = sm.OLS(y, X).fit()
```

- ✅ נותן לך פלט שלם: p-values, R², Adjusted R², סטיות תקן
- ✅ תומך גם בפולינום (אם אתה מוסיף ידנית עמודות X², X³ וכו')
- ❌ דורש ממך להוסיף bias (עמודת 1) בעצמך עם `sm.add_constant`
- ❌ פחות אינטואיטיבי כשמשתמשים בפיצ’רים מרובים

💡 **בחירה מושלמת אם אתה רוצה ניתוח סטטיסטי מפורט ומדויק, כמו מאמר מדעי.**

---

## 📊 סיכום השוואתי

| Library                  | Supports Polynomial? | Supports R²? | Supports p-values? | Best Suited For                        |
|--------------------------|------------------|--------------|--------------------|----------------------------------------|
| `sklearn.linear_model`   | ✅ (עם `PolynomialFeatures`) | ✅ | ❌ | Machine learning ו־pipeline             |
| `numpy.polyfit`          | ✅ (פיצ’ר אחד בלבד)         | ❌ (צריך לחשב בנפרד) | ❌ | גרפים פשוטים ומהירים                  |
| `statsmodels.OLS`        | ✅ (ידנית)                  | ✅ | ✅ | ניתוח סטטיסטי מתקדם (כמו מאמרים)       |
---

## 📘 5. `Ridge` (Regularized Linear Regression - L2)

```python
from sklearn.linear_model import Ridge
```

- ✅ מבצעת רגרסיה לינארית עם רגולריזציה מסוג L2 (עונש על סכום ריבועי המקדמים)
- ✅ טובה למקרים עם הרבה פיצ'רים או קולינאריות גבוהה
- ❌ מקטינה את המקדמים אבל לא מבטלת אותם לגמרי

💡 **מעולה כשיש הרבה משתנים והקשר ביניהם בעייתי או חופף.**

---

## 📘 6. `Lasso` (Regularized Linear Regression - L1)

```python
from sklearn.linear_model import Lasso
```

- ✅ רגולריזציה מסוג L1 (עונש על סכום ערכי מוחלט של המקדמים)
- ✅ מבטלת משתנים פחות חשובים – אפקט של feature selection
- ❌ עלולה לאבד מידע חשוב אם הנתונים רועשים מדי

💡 **בחירה נהדרת כשאתה רוצה גם רגרסיה וגם לבחור משתנים רלוונטיים.**

---

## 📊 סיכום מעודכן:

| ספרייה                  | תומכת בפולינום? | תומכת ב־R²? | תומכת ב־p-values? | רגולריזציה | הכי מתאימה ל־                        |
|--------------------------|------------------|--------------|--------------------|--------------|----------------------------------------|
| `sklearn.linear_model`   | ✅ (with `PolynomialFeatures`) | ✅ | ❌ | ❌           | ML workflows and pipelines             |
| `numpy.polyfit`          | ✅ (single feature only)         | ❌ (manual calculation needed) | ❌ | ❌           | Simple and quick curve fitting         |
| `statsmodels.OLS`        | ✅ (manual expansion)            | ✅ | ✅ | ❌           | Advanced statistical analysis          |
| `Ridge`                  | ✅ (with `PolynomialFeatures`)   | ✅ | ❌ | ✅ (L2)     | Handles multicollinearity and improves generalization |
| `Lasso`                  | ✅ (with `PolynomialFeatures`)   | ✅ | ❌ | ✅ (L1)     | Automatic feature selection and sparsity |
