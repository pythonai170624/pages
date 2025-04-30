# K-MEAN CLUSTERING - INTRODUCTION

**קיי מינס** הוא אלגוריתם של **למידה לא מונחית** שמטרתו לחלק את הנתונים לקבוצות (קלאסטרים) לפי הדמיון בין נקודות  
הוא לא יודע מראש כמה קבוצות יש או מה משמעותן – רק מחלק את הנקודות כך שכל קבוצה תהיה פנימית הומוגנית וחיצונית שונה  

🧠 **האלגוריתם לא יודע להסביר מהי כל קבוצה**, זה התפקיד של מנתח הדאטה  
לכן הוא קורא לקבוצות פשוט בשם **group_1**, **group_2** וכו’  

---

# K-MEAN CLUSTERING - ALGORITHM STEPS

## 1. Initialization  
**אתחול** – בוחרים את מספר הקבוצות **K** וממקמים נקודות התחלתיות (מרכזים) בצורה אקראית במרחב

## 2. Assignment  
**שיוך** – כל נקודה בדאטה משויכת למרכז הקרוב ביותר לפי **מרחק אוקלידי**

## 3. Update  
**עדכון מרכזים** – מחשבים מחדש את מיקום כל מרכז בתור **ממוצע** של כל הנקודות שנמצאות בקבוצה הזו

## 4. Reassignment  
**שיוך מחדש** – מבצעים בדיקה נוספת: אם יש נקודות שהתקרבו יותר למרכז אחר, מעבירים אותן לקבוצה החדשה

## 5. Repeat  
**חזרה** – חוזרים על השלבים הקודמים עד שאין שינוי בין האיטרציות או שהגענו למספר מקסימלי של חזרות


---

# K-MEAN CLUSTERING - EXAMPLE

בוא נראה דוגמה:

- יש לנו דאטה עם שני מאפיינים: **X1** ו־**X2**
- נבחר K=3 כלומר נרצה שלוש קבוצות

🖼️ שלב ראשון: האלגוריתם בוחר 3 נקודות התחלתיות באופן אקראי  
להלן תמונה הממחישה את המצב הזה:

<img src="data:image/png;base64,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" />


hi