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

---

### עבור רגרסיה (Regression):

1. **Mean Squared Error (MSE)**  
   - ממוצע ריבועי השגיאות

2. **Mean Absolute Error (MAE)**  
   - ממוצע ערכי השגיאות המוחלטים

3. **R² Score (מקדם הדטרמינציה)**  
   - מודד כמה טוב המודל מסביר את השונות בנתונים

---

## דוגמה בקוד (סיווג)

```python
from sklearn.datasets import load_iris
from sklearn.tree import DecisionTreeClassifier
from sklearn.model_selection import train_test_split
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score, confusion_matrix

# טוענים את הדאטה
X, y = load_iris(return_X_y=True)

# מחלקים לסט אימון וסט בדיקה
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=42)

# בונים עץ החלטה
clf = DecisionTreeClassifier()
clf.fit(X_train, y_train)

# מנבאים על סט הבדיקה
y_pred = clf.predict(X_test)

# מחשבים מטריקות
print("Accuracy:", accuracy_score(y_test, y_pred))
print("Precision (macro):", precision_score(y_test, y_pred, average='macro'))
print("Recall (macro):", recall_score(y_test, y_pred, average='macro'))
print("F1 Score (macro):", f1_score(y_test, y_pred, average='macro'))
print("Confusion Matrix:\n", confusion_matrix(y_test, y_pred))
