# 🎯 המשך הערכת מודל רגרסיה לוגיסטית

---

## ❗ פרדוקס הדיוק (Accuracy Paradox)

לפעמים מדד הדיוק **מטעה** – במיוחד כשיש **חוסר איזון בנתונים**.

### 🔍 דוגמה:
נניח שבמאגר שלנו:
- 95 מתוך 100 האנשים **לא חולים**
- רק 5 חולים

המודל "מתחכם" ומנבא שכולם לא חולים (כלומר תמיד 0).

#### 🎯 תוצאה:
- Accuracy = 95%
- אבל המודל **אף פעם לא מזהה חולה אמיתי**
- כלומר המודל חסר תועלת למרות דיוק גבוה

---

## 📢 Recall (שחזור)

מודד כמה מהמקרים **החיוביים האמיתיים** המודל הצליח לגלות.

$$
\text{Recall} = \frac{TP}{TP + FN}
$$

- TP = חיזוי חיובי נכון  
- FN = פספוס של חיובי

### 🧠 דוגמה:
אם יש 5 חולים, והמודל גילה 4 →  

$$
\text{Recall} = \frac{4}{5} = 0.8
$$

---

## 🎯 Precision (דיוק תחזיות חיוביות)

מודד כמה מהתחזיות שהמודל חזה כ"חיובי" הן באמת נכונות.

$$
\text{Precision} = \frac{TP}{TP + FP}
$$

- FP = חיזוי חיובי שגוי (False Alarm)

### 🧠 דוגמה:
אם המודל חזה 6 חיוביים, אבל רק 4 באמת חולים →  
$$
\text{Precision} = \frac{4}{6} \approx 0.666
$$

---

## ⚖️ F1 Score – ממוצע הרמוני של Precision ו-Recall

מאזן בין Precision ו-Recall. חשוב במיוחד כשיש חוסר איזון בין המקרים.

$$
\text{F1 Score} = 2 \cdot \frac{\text{Precision} \cdot \text{Recall}}{\text{Precision} + \text{Recall}}
$$

### 🧠 דוגמה:
Precision = 0.666, Recall = 0.8

$$
\text{F1} = \frac{2 \cdot 0.666 \cdot 0.8}{0.666 + 0.8} \approx 0.72
$$

---

## 🌡️ הצגת המודל עם Heatmap

מטריצת בלבול (Confusion Matrix) היא טבלה –  
אבל ניתן להמחיש אותה בצורה ברורה יותר עם **מפת חום (Heatmap)**.

### 🔥 דוגמה בפייתון:

```python
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.metrics import confusion_matrix

y_true = [1, 0, 1, 1, 0, 0, 1]
y_pred = [1, 0, 0, 1, 0, 1, 1]

cm = confusion_matrix(y_true, y_pred)

sns.heatmap(cm, annot=True, fmt="d", cmap="Blues")
plt.xlabel("Predicted")
plt.ylabel("Actual")
plt.title("Confusion Matrix - Heatmap")
plt.show()
