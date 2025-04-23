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
  
## סיכום :
  
- ב-**CART**:
  - **Classification** → משתמשים ב-**Gini Impurity**.
  - **Regression** → משתמשים ב-**MSE** כדי לבחור את הפיצול הכי טוב.
- **מטרה:** לבחור את הפיצול שמפחית הכי הרבה את ה-MSE → עוזר לנבא ערכים מספריים בצורה מדויקת יותר.
  
  