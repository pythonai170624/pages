# השוואה בין Gini Impurity ל-Entropy
  
## נוסחאות:
  
### 1. Gini Impurity:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Gini%20=%201%20-%20\sum_{i=1}^{n}%20p_i^2"/></p>  
  
  
כאשר:
- <img src="https://latex.codecogs.com/gif.latex?p_i"/> הוא ההסתברות של כל קטגוריה <img src="https://latex.codecogs.com/gif.latex?i"/> בקבוצה.
  
---
  
### 2. Entropy:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy%20=%20-%20\sum_{i=1}^{n}%20p_i%20\cdot%20\log_2(p_i)"/></p>  
  
  
כאשר:
- <img src="https://latex.codecogs.com/gif.latex?p_i"/> הוא ההסתברות של כל קטגוריה <img src="https://latex.codecogs.com/gif.latex?i"/> בקבוצה.
  
---
  
## דוגמה מספרית:
  
נניח שיש לנו קבוצה עם שתי קטגוריות:
- חיובי (Positive) עם הסתברות <img src="https://latex.codecogs.com/gif.latex?p_1%20=%200.7"/>
- שלילי (Negative) עם הסתברות <img src="https://latex.codecogs.com/gif.latex?p_2%20=%200.3"/>
  
### חישוב Gini:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Gini%20=%201%20-%20(0.7^2%20+%200.3^2)%20=%201%20-%20(0.49%20+%200.09)%20=%200.42"/></p>  
  
  
### חישוב Entropy:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy%20=%20-%20(0.7%20\cdot%20\log_2(0.7)%20+%200.3%20\cdot%20\log_2(0.3))%20\\Entropy%20=%20-%20(0.7%20\cdot%20-0.5146%20+%200.3%20\cdot%20-1.737)%20\\Entropy%20\approx%20-%20(-0.3602%20-%200.5211)%20=%200.8813"/></p>  
  
  
---
  
## מתי להשתמש?
  
| מאפיין            | Gini Impurity           | Entropy                  |
|-------------------|-------------------------|--------------------------|
| חישוביות          | מהיר יותר (ללא לוגריתם) | איטי יותר (יש לוגריתם)   |
| רגישות            | פחות רגיש להבדלים קטנים | יותר רגיש להבדלים קטנים  |
| מתי לבחור         | כשחשובה מהירות          | כשחשובה דיוק/מידע         |
| שימוש נפוץ        | Decision Tree (CART)    | Decision Tree (ID3, C4.5)|
  
**המלצה כללית:**
- אם יש לך הרבה נתונים או אתה צריך חישוב מהיר → **Gini**  
- אם אתה רוצה להיות רגיש יותר לאי-ודאות → **Entropy**
  
## CART, ID3, C4.5

### 1. CART (Classification And Regression Tree)

- **מה זה?** אלגוריתם שמייצר עץ החלטה לסיווג (Classification) או חיזוי ערכים מספריים (Regression).
- **מדד חלוקה:** 
  - ב-Classification: משתמש ב-**Gini Impurity**.
  - ברגרסיה: משתמש ב-**Mean Squared Error (MSE)**.
- **מאפיינים:**
  - תמיד מפצל ל-2 קבוצות (Binary Split).
  - מתאים גם לסיווג וגם לרגרסיה.


#### Information Gain
Information Gain is a metric used to select the best attribute for splitting in a decision tree. It measures the reduction in entropy (uncertainty) achieved by splitting the data according to a particular attribute.

Information Gain is defined as:
```
InformationGain(S, A) = Entropy(S) - Σ (|Sv| / |S|) * Entropy(Sv)
```

Where:
- S is the current set of examples
- A is the attribute being tested
- Sv is the subset of S with value v for attribute A
- Entropy(S) = -Σ p(i) * log₂(p(i)), where p(i) is the proportion of examples from class i in set S

Higher Information Gain indicates an attribute that reduces more uncertainty in the data, and is therefore preferred for splitting.


---

## 2. ID3 (Iterative Dichotomiser 3)

- **מה זה?** אלגוריתם ליצירת עצי החלטה **לסיווג בלבד** (לא רגרסיה).
- **מדד חלוקה:** 
  - משתמש ב-**Entropy** וב-**Information Gain** כדי לבחור את הפיצ'ר הכי טוב לפיצול.
- **מאפיינים:**
  - יכול לפצל ליותר מ-2 קבוצות (לא רק בינארי).
  - לא תומך בנתונים חסרים.
  - מתאים רק לקטגוריות דיסקרטיות (לא תומך במספרים רציפים).

---

## 3. C4.5

- **מה זה?** שדרוג של ID3, גם לסיווג בלבד.
- **מדד חלוקה:** 
  - משתמש ב-**Gain Ratio** (שיפור של Information Gain כדי להתמודד עם בעיות פיצול לא הוגן).
- **מאפיינים:**
  - כן תומך במספרים רציפים (יודע לחלק אותם לפי סף).
  - תומך בנתונים חסרים.
  - מפסיק לבנות עץ כשהוא לא מוצא פיצול טוב (Pruning - גיזום).

---

## השוואה קצרה:

| תכונה              | CART                  | ID3                    | C4.5                 |
|--------------------|-----------------------|------------------------|----------------------|
| שימוש              | סיווג ורגרסיה         | סיווג בלבד             | סיווג בלבד          |
| מדד חלוקה          | Gini / MSE            | Entropy + Info Gain    | Gain Ratio           |
| תומך במספרים רציפים| כן                    | לא                     | כן                   |
| תומך בנתונים חסרים | כן                    | לא                     | כן                   |
| סוג הפיצול         | בינארי בלבד           | לא חייב בינארי         | לא חייב בינארי       |

---
  
## גרפים (במילים 😅):
  
- **Gini** מקסימלי כשההסתברויות שוות (0.5, 0.5).
- **Entropy** גם מקסימלי כשההסתברויות שוות, אבל הערך שלו גדול יותר (1 לעומת 0.5 ב-Gini לשני מחלקות).
  
  