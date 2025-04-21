## 📘 הסבר: מה זה <img src="https://latex.codecogs.com/gif.latex?\beta_{j1},%20\dots,%20\beta_{jn}"/> בפונקציית Softmax?
  
כאשר משתמשים במודל Softmax לסיווג רב-קטגורי (Multiclass), כל קטגוריה <img src="https://latex.codecogs.com/gif.latex?j"/> מקבלת **וקטור משקלים** משלה.
  
---
  
### 💡 הגדרה:
  
- <img src="https://latex.codecogs.com/gif.latex?\beta_{ji}"/>: זהו **המשקל של התכונה ה-<img src="https://latex.codecogs.com/gif.latex?i"/>** עבור **הקטגוריה ה-<img src="https://latex.codecogs.com/gif.latex?j"/>**.
- כלומר, עבור כל קטגוריה <img src="https://latex.codecogs.com/gif.latex?j"/>, יש סדרה של משקלים:
  <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_{j1},%20\beta_{j2},%20\dots,%20\beta_{jn}"/></p>  
  
  שמתאימים לתכונות <img src="https://latex.codecogs.com/gif.latex?x_1,%20x_2,%20\dots,%20x_n"/> של הדוגמה.
  
---
  
### 🧠 למה צריך את זה?
  
במודל Softmax, אנחנו רוצים לחשב "ציון" <img src="https://latex.codecogs.com/gif.latex?z_j"/> לכל קטגוריה <img src="https://latex.codecogs.com/gif.latex?j"/>, ולכן כל קטגוריה צריכה את **מערכת המשקלים שלה**, כדי להתאים לתכונות של הדוגמה:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?z_j%20=%20\beta_{j0}%20+%20\beta_{j1}x_1%20+%20\beta_{j2}x_2%20+%20\dots%20+%20\beta_{jn}x_n"/></p>  
  
  
- <img src="https://latex.codecogs.com/gif.latex?\beta_{j0}"/>: זהו ה־**bias** (היסט) של הקטגוריה <img src="https://latex.codecogs.com/gif.latex?j"/>
- <img src="https://latex.codecogs.com/gif.latex?x_i"/>: אלו **ערכי התכונות** של הדוגמה שאנחנו רוצים לסווג
  
---
  
### 📊 דוגמה:
  
אם יש לנו 3 קטגוריות (נגיד: חתול 🐱, כלב 🐶, תוכי 🦜), ו-4 תכונות <img src="https://latex.codecogs.com/gif.latex?x_1%20\dots%20x_4"/>, אז:
  
- ל־**חתול** יהיו:  
  <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_{cat0},%20\beta_{cat1},%20\beta_{cat2},%20\beta_{cat3},%20\beta_{cat4}"/></p>  
  
  
- ל־**כלב** יהיו:  
  <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_{dog0},%20\beta_{dog1},%20\beta_{dog2},%20\beta_{dog3},%20\beta_{dog4}"/></p>  
  
  
- ל־**תוכי** יהיו:  
  <p align="center"><img src="https://latex.codecogs.com/gif.latex?\beta_{parrot0},%20\beta_{parrot1},%20\beta_{parrot2},%20\beta_{parrot3},%20\beta_{parrot4}"/></p>  
  
  
כל אחת מהקטגוריות משתמשת במשקלים שלה כדי לחשב ציון שמתורגם להסתברות בעזרת פונקציית Softmax.
  
---
  
### ✅ סיכום:
  
> <img src="https://latex.codecogs.com/gif.latex?\beta_{j1}%20\dots%20\beta_{jn}"/> הם **המשקלים האישיים של הקטגוריה ה-<img src="https://latex.codecogs.com/gif.latex?j"/>** עבור כל אחת מהתכונות.  
> ככל שהתכונה רלוונטית יותר לקטגוריה — המשקל שלה יהיה גבוה יותר.
  
  