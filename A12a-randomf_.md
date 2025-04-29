# יער רנדומלי (Random Forest)
  
## הבעיה שרוצים לפתור
  
עצי החלטה בודדים סובלים מבעיות כמו:
- **Overfitting** — העץ לומד טוב מדי את הדאטה ולא מסוגל לחזות על דאטה חדש
- **התעלמות מתכונות** — העץ מתמקד רק בחלק מהתכונות בפיצולים
  
**דוגמה**:
  
נניח שאנו רוצים לחזות אם לקוח יקבל אשראי, בהתבסס על גיל ומשכורת. עץ החלטה יחיד עשוי לבנות חוקים מדויקים מאוד לדאטה הקיים, אך ייכשל לחזות נכונה לקוחות חדשים
  
---
  
## איך Random Forest פותר את הבעיה?
  
במקום לבנות **עץ אחד**, יער רנדומלי בונה **הרבה עצים שונים** על **חלקים שונים מהמידע** ו**חלקים שונים מהתכונות**:
  
- בבעיית סיווג (Classification) → לוקחים **רוב קולות** (Voting)
- בבעיית רגרסיה (Regression) → לוקחים **ממוצע** של התחזיות
  
---
  
## תהליך פעולה של Random Forest
  
1. בחירה **מדגם אקראי** מהדאטה (Bootstrapping)
2. בכל פיצול: **בחירה אקראית** של קבוצת תכונות
3. בניית עץ עד הסוף (ללא גיזום)
4. חזרה על השלבים כדי לבנות הרבה עצים
5. תחזית סופית על ידי רוב קולות או ממוצע
  
---
  
## מה זה Bootstrapping במודל?
  
 תהליך שבו אנו יוצרים מדגם חדש מהנתונים הקיימים **על ידי בחירה אקראית של דגימות עם החזרה**. כלומר:
- ייתכן שדוגמה תיבחר יותר מפעם אחת
- ייתכן שדוגמה מסוימת לא תיבחר בכלל
  
המטרה: כל עץ רואה גרסה מעט שונה של הדאטה. זה מייצר **גיוון** בין העצים ועוזר למודל להיות יותר יציב ולפחות מוטה.
  
**דוגמה פשוטה**:
  
אם יש לנו 5 דוגמאות: A, B, C, D, E
  
אחד העצים יכול להיבנות על בסיס מדגם כמו: A, B, B, D, E
  
---
  
## מתמטיקה בסיסית
  
אם:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?T_1(x),%20T_2(x),%20\ldots,%20T_K(x)"/></p>  
  
הם העצים,
  
אז התחזית הסופית היא:
  
- **סיווג**:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\text{Prediction}(x)%20=%20\text{Majority%20Vote}(T_1(x),%20T_2(x),%20\ldots,%20T_K(x))"/></p>  
  
  
- **רגרסיה**:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\text{Prediction}(x)%20=%20\frac{1}{K}%20\sum_{i=1}^{K}%20T_i(x)"/></p>  
  
  
---
  
## השוואה: עץ החלטה יחיד מול Random Forest
  
| נושא | עץ החלטה בודד | יער רנדומלי |
|:-----|:--------------|:------------|
| דיוק | גבוה על אימון, נמוך על חדש | יציב וגבוה |
| שימוש בתכונות | חלק מהתכונות מוזנחות | רוב התכונות משולבות |
| שונות | גבוהה | נמוכה |
  
---
  
## פרמטרים חשובים ב-Random Forest
  
- `n_estimators` → מספר העצים לבנות
- `max_features` → מספר התכונות לבדוק בכל פיצול
- `bootstrap` → האם לדגום עם החזרה
- `oob_score` → חישוב Out-Of-Bag Error לאמדן ביצועים
  
---
  
## למה Bootstrapping חשוב?
  
- יוצר גיוון בין העצים
- מפחית קורלציה בין עצים
- משפר יציבות והכללה (generalization)
  
---
  
## דוגמת קוד פשוטה בפייתון
  
```python
from sklearn.ensemble import RandomForestClassifier
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score
  
# טוענים דאטה
X, y = load_iris(return_X_y=True)
  
# פיצול ל-train ו-test
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)
  
# יצירת מודל Random Forest
model = RandomForestClassifier(n_estimators=100, random_state=42)
model.fit(X_train, y_train)
  
# חיזוי על סט הבדיקה
y_pred = model.predict(X_test)
  
# חישוב דיוק
accuracy = accuracy_score(y_test, y_pred)
print(f"דיוק המודל: {accuracy:.2f}")
```
  
פלט אפשרי:
```
דיוק המודל: 1.00
```
  
---
  
## גרף המחשה
  
<img src="https://upload.wikimedia.org/wikipedia/commons/7/76/Random_forest_diagram_complete.png" width="600">
  
**הסבר:**
- הדאטה מפוצל על עצים שונים
- בסוף אוספים את כל התחזיות ומקבלים החלטה סופית
  
---
  
## ✨ סיכום ראשוני
  
- יער רנדומלי = הרבה עצי החלטה שונים
- מתאים לסיווג ולרגרסיה
- מפחית Overfitting ומשפר דיוק
- דורש יותר זמן חישוב, אבל מאוד אמין וביציב
  
---
  
  