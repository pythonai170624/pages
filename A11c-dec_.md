# השוואה בין Gini Impurity ל-Entropy
  
## נוסחאות:
  
### 1. Gini Impurity:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Gini%20=%201%20-%20/sum_{i=1}^{n}%20p_i^2"/></p>  
  
  
כאשר:
- <img src="https://latex.codecogs.com/gif.latex?p_i"/> הוא ההסתברות של כל קטגוריה <img src="https://latex.codecogs.com/gif.latex?i"/> בקבוצה.
  
### 2. Entropy:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy%20=%20-%20/sum_{i=1}^{n}%20p_i%20/cdot%20/log_2(p_i)"/></p>  
  
  
כאשר:
- <img src="https://latex.codecogs.com/gif.latex?p_i"/> הוא ההסתברות של כל קטגוריה <img src="https://latex.codecogs.com/gif.latex?i"/> בקבוצה.
  
## דוגמה מספרית:
  
נניח שיש לנו קבוצה עם שתי קטגוריות:
- חיובי (Positive) עם הסתברות <img src="https://latex.codecogs.com/gif.latex?p_1%20=%200.7"/>
- שלילי (Negative) עם הסתברות <img src="https://latex.codecogs.com/gif.latex?p_2%20=%200.3"/>
  
### חישוב Gini:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Gini%20=%201%20-%20(0.7^2%20+%200.3^2)%20=%201%20-%20(0.49%20+%200.09)%20=%200.42"/></p>  
  
  
### חישוב Entropy:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy%20=%20-%20(0.7%20/cdot%20/log_2(0.7)%20+%200.3%20/cdot%20/log_2(0.3))%20//Entropy%20=%20-%20(0.7%20/cdot%20-0.5146%20+%200.3%20/cdot%20-1.737)%20//Entropy%20/approx%20-%20(-0.3602%20-%200.5211)%20=%200.8813"/></p>  
  
  
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
  
---  
  
## מהו CART, ID3, C4.5
  
### 1. CART (Classification And Regression Tree)
  
- **מה זה?** אלגוריתם שמייצר עץ החלטה לסיווג (Classification) או חיזוי ערכים מספריים (Regression).
- **מדד חלוקה:** 
  - ב-Classification: משתמש ב-**Gini Impurity**
  - ברגרסיה: משתמש ב-**Mean Squared Error (MSE)**
- **מאפיינים:**
  - תמיד מפצל ל-2 קבוצות (Binary Split)
  - מתאים גם לסיווג וגם לרגרסיה
  
  
#### מה זה Information Gain?
  
מדד שמראה **כמה מידע הרווחנו** (או כמה אי-ודאות הפחתנו) כשאנחנו מחלקים קבוצה לפי פיצ'ר מסוים
  
##### למה צריך את זה?
  
כשבונים **עץ החלטה** (למשל ב-ID3 או C4.5), רוצים לבחור את הפיצ'ר **הכי טוב לפיצול** הדאטה
הפיצ'ר עם ה-**Information Gain** הכי גבוה הוא זה שמפחית הכי הרבה את אי-הוודאות (כלומר עושה את הקבוצה הכי "טהורה")
  
##### איך מחשבים את זה?
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Information/%20Gain%20=%20Entropy_{before}%20-%20Entropy_{after}"/></p>  
  
  
- **Entropy_before**: כמות אי-הוודאות לפני החלוקה
- **Entropy_after**: כמות אי-הוודאות הממוצעת אחרי החלוקה (לפי כל קבוצה שנוצרה)
  
##### דוגמה:
  
נניח שיש לנו 10 דוגמאות:
- 6 **כן** (קנה מוצר)
- 4 **לא** (לא קנה מוצר)
  
**1. מחשבים Entropy לפני החלוקה:**
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy_{before}%20=%20-%20(0.6%20/cdot%20/log_2(0.6)%20+%200.4%20/cdot%20/log_2(0.4))%20///approx%200.9709"/></p>  
  
  
**2. מחלקים לפי פיצ'ר (למשל גיל):**
  
**קבוצה 1 (גיל < 30):**
  
- 4 כן
- 1 לא
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy_1%20=%20-%20(0.8%20/cdot%20/log_2(0.8)%20+%200.2%20/cdot%20/log_2(0.2))%20///approx%200.7219"/></p>  
  
  
**קבוצה 2 (גיל >= 30):**
  
- 2 כן
- 3 לא
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy_2%20=%20-%20(0.4%20/cdot%20/log_2(0.4)%20+%200.6%20/cdot%20/log_2(0.6))%20///approx%200.9709"/></p>  
  
**3. מחשבים את ה-Entropy אחרי החלוקה:**
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Entropy_{after}%20=%20/frac{5}{10}%20/cdot%20Entropy_1%20+%20/frac{5}{10}%20/cdot%20Entropy_2%20//=%200.5%20/cdot%200.7219%20+%200.5%20/cdot%200.9709%20//=%200.8464"/></p>  
  
  
**4. מחשבים את Information Gain:**
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?Information/%20Gain%20=%200.9709%20-%200.8464%20=%200.1245"/></p>  
  
**מה זה אומר?**
  
- אם **IG גבוה** → הפיצ'ר הזה עוזר **להפחית את אי-הוודאות** → כדאי לבחור בו
- אם **IG נמוך** → הפיצ'ר כמעט לא משפיע → פחות כדאי לבחור בו
  
**טיפ:**
  
- ב-ID3 → תמיד בוחרים את הפיצ'ר עם ה-**IG הכי גבוה**
- ב-C4.5 → משתמשים ב-**Gain Ratio** (שיפור על IG) כדי למנוע העדפה לפיצ'רים עם הרבה קבוצות
  
  
# מה זה Mean Squared Error (MSE) בעצי החלטה?
  
כשמשתמשים ב-**CART** לבעיות **רגרסיה** (כלומר חיזוי ערך מספרי ולא סיווג), צריך **מדד טוהר** שמתאים לנתונים רציפים.  
במקום **Gini Impurity** (שמתאים לקטגוריות), משתמשים ב-**Mean Squared Error (MSE)**.
  
---
  
## איך זה עובד?
  
### MSE מחשב:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?MSE%20=%20\frac{1}{n}%20\sum_{i=1}^{n}%20(y_i%20-%20\hat{y})^2"/></p>  
  
  
- <img src="https://latex.codecogs.com/gif.latex?y_i"/> = הערך האמיתי של כל דוגמה.
- <img src="https://latex.codecogs.com/gif.latex?\hat{y}"/> = הערך הממוצע בקבוצה (התחזית).
- <img src="https://latex.codecogs.com/gif.latex?n"/> = מספר הדוגמאות בקבוצה.
  
**מטרה:** לבחור את הפיצול ש*מפחית* הכי הרבה את ה-MSE.
  
---
  
## דוגמה פשוטה:
  
נניח יש לנו נתוני מכירות (באלפים):
  
| גיל | מכירות |
|-----|--------|
| 25  | 50     |
| 30  | 60     |
| 35  | 65     |
| 40  | 80     |
  
### 1. מחשבים את ה-MSE לפני פיצול:
- הממוצע של כל המכירות:  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{y}%20=%20\frac{50%20+%2060%20+%2065%20+%2080}{4}%20=%2063.75"/></p>  
  
- מחשבים את ה-MSE:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?MSE%20=%20\frac{1}{4}%20\left(%20(50-63.75)^2%20+%20(60-63.75)^2%20+%20(65-63.75)^2%20+%20(80-63.75)^2%20\right)%20\\=%20\frac{1}{4}%20(189.06%20+%2014.06%20+%201.56%20+%20264.06)%20\\=%20\frac{1}{4}%20\cdot%20468.75%20=%20117.19"/></p>  
  
  
---
  
### 2. בודקים פיצול (למשל גיל < 32):
  
#### קבוצה 1 (גיל < 32):
- מכירות: 50, 60
- ממוצע:  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{y}_1%20=%20\frac{50%20+%2060}{2}%20=%2055"/></p>  
  
- MSE:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?MSE_1%20=%20\frac{1}{2}%20\left(%20(50-55)^2%20+%20(60-55)^2%20\right)%20=%2025"/></p>  
  
  
#### קבוצה 2 (גיל >= 32):
- מכירות: 65, 80
- ממוצע:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\hat{y}_2%20=%20\frac{65%20+%2080}{2}%20=%2072.5"/></p>  
  
- MSE:
<p align="center"><img src="https://latex.codecogs.com/gif.latex?MSE_2%20=%20\frac{1}{2}%20\left(%20(65-72.5)^2%20+%20(80-72.5)^2%20\right)%20=%2056.25"/></p>  
  
  
---
  
### 3. מחשבים את ה-MSE הכולל אחרי הפיצול:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?MSE_{after}%20=%20\frac{2}{4}%20\cdot%20MSE_1%20+%20\frac{2}{4}%20\cdot%20MSE_2%20\\=%200.5%20\cdot%2025%20+%200.5%20\cdot%2056.25%20=%2040.625"/></p>  
  
  
---
  
### 4. בודקים את השיפור:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\Delta%20MSE%20=%20MSE_{before}%20-%20MSE_{after}%20\\=%20117.19%20-%2040.625%20=%2076.56"/></p>  
  
  
- **המסקנה:** הפיצול הזה מוריד את ה-MSE בצורה משמעותית → כדאי לבחור בו!
  
---
  
## סיכום 
  
- ב-**CART**:
  - **Classification** → משתמשים ב-**Gini Impurity**.
  - **Regression** → משתמשים ב-**MSE** כדי לבחור את הפיצול הכי טוב.
- **מטרה:** לבחור את הפיצול שמפחית הכי הרבה את ה-MSE → עוזר לנבא ערכים מספריים בצורה מדויקת יותר.
  
  
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
  
  
  
  