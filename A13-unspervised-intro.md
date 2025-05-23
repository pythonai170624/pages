# What is Unsupervised Learning?

ב**למידה לא מונחית** אנחנו עובדים רק עם המאפיינים של הדאטה – בלי תוויות  
כלומר, אין לנו תשובה נכונה מראש כמו שיש ב**למידה מונחית**  
האלגוריתם מקבל את הנתונים ומנסה למצוא בהם מבנים, תבניות או קבוצות  
המטרה היא להבין מה קורה בתוך הדאטה גם בלי שמישהו יגיד לנו מה נכון ומה לא  
קשה מאוד להעריך את הדיוק של המודל כי אין לנו למה להשוות את התוצאה  

---

# Tasks in Unsupervised Learning

בדיוק כמו שב**למידה מונחית** חילקנו את המודלים לבעיות של **סיווג** ו**רגרסיה**  
גם כאן יש חלוקה לסוגי משימות עיקריים

## Clustering

**קלאסטרינג** מחלק את הדאטה לקבוצות דומות לפי מאפיינים  
למשל: חלוקת לקוחות לפי התנהגות קנייה, גם בלי לדעת מיהם מראש

## Association

**אסוציאציה** מחפשת קשרים וחוקים בתוך הנתונים  
משתמשים בזה הרבה בניתוח סלי קנייה – לראות אילו פריטים מופיעים יחד

## Dimensionality Reduction

**הפחתת ממדים** משמשת כשהדאטה כולל יותר מדי מאפיינים  
המטרה היא לצמצם את הכמות ועדיין לשמור על המשמעות המרכזית  
זה עוזר להאיץ את זמן הלמידה ולעשות ויזואליזציה נוחה יותר

---

# LABELED DATA

ההבדל המרכזי בין **למידה מונחית** ל**למידה לא מונחית** הוא קיום תוויות  
ב**למידה מונחית** האלגוריתם מקבל גם את הקלט וגם את הפלט הרצוי  
ב**למידה לא מונחית** יש רק קלט – והמודל צריך להבין לבד איך הנתונים מתחלקים  
המודל לא יודע מה נכון ומה שגוי אלא מנסה למצוא סדר כלשהו במידע  

---

# MODEL GOALS

ב**למידה מונחית** אנחנו רוצים לחזות תוצאה חדשה  
לדוגמה: אם סטודנט יקבל ציון עובר או לא  
ב**למידה לא מונחית** אנחנו רוצים להבין את הדאטה, למצוא בו תבניות, קבוצות או מבנים שלא הכרנו  
למשל: חלוקה של מוצרים באתר לפי הרגלי גלישה של המשתמשים  

---

# COMPLEXITY

**למידה מונחית** לרוב פשוטה יותר ליישום  
מספיק להשתמש בכלים רגילים כמו **פייתון** או **ר** ולעבוד עם מערכי נתונים בגודל סביר  
ב**למידה לא מונחית** צריך לעיתים קרובות יותר כוח מחשוב גבוה כי עובדים עם כמויות גדולות של נתונים לא מסווגים  
החישובים מורכבים יותר כי אין למה להשוות  

---

# DRAWBACKS

החיסרון של **למידה מונחית** הוא הצורך לתייג את הנתונים – דבר שלוקח זמן ודורש מומחיות  
ב**למידה לא מונחית** יש סיכון גבוה שהתוצאה לא תהיה מדויקת, כי אין דרך לבדוק אם הפלט שקיבלנו הגיוני  
צריך לרוב התערבות אנושית כדי לפרש את התוצאות  
