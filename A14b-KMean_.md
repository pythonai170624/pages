# Optimal K Value
  
בשלב הזה אנחנו רוצים לבחור את מספר הקבוצות הטוב ביותר למודל שלנו  
עד עכשיו בחרנו את המספר קיי בצורה אקראית, אבל עכשיו ננסה לבחור אותו בצורה חכמה  
בשונה מלמידה עם מורה, כאן אין לנו תשובה נכונה להשוות אליה – אז אי אפשר לחשב דיוק רגיל  
לכן נשתמש במדד פנימי שמודד עד כמה הנקודות קרובות למרכז הקבוצה שלהן
  
### Sum of Squared Distances (SSD)
  
**סכום ריבועי המרחקים** הוא מדד פנימי שמשמש אותנו לבדוק עד כמה הקבוצות שיצרנו טובות  
בכל קבוצה אנחנו מחשבים את המרחק של כל נקודת דאטה מהמרכז של הקבוצה שלה – ואז מעלים את המרחק הזה בריבוע  
עושים את זה לכל הנקודות בכל הקבוצות – ובסוף מחברים את כל הערכים האלה  
ככל שהסכום הזה קטן יותר – סימן שהנקודות קרובות יותר למרכזים, כלומר הקבוצות **מהודקות** וטובות
  
📌 **למה מעלים בריבוע?**  
כדי להעניש מרחקים גדולים יותר – אם יש נקודה שחרגה מאוד, היא תשפיע הרבה יותר על המדד
  
📌 **מתי משתמשים בזה?**  
כאשר אין לנו תשובות נכונות להשוות אליהן, כמו בלמידה לא מונחית – זו הדרך לבדוק אם המודל שלנו מתפקד טוב
  
📌 **דוגמה פשוטה:**  
אם יש קבוצה של 5 נקודות שכולן קרובות למרכז – אז הסכום יהיה נמוך  
אם בקבוצה אחרת הנקודות מפוזרות – הסכום יהיה גבוה יותר, והמחשב יבין שהקבוצה הזו פחות טובה
  
#### Formula
  
הנוסחה הכללית לחישוב סכום ריבועי המרחקים היא:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?S%20=%20\sum_{i=1}^{n}%20\|x_i%20-%20\mu_{c_i}\|^2"/></p>  
  
  
**Where:**
- <img src="https://latex.codecogs.com/gif.latex?S"/> is the total sum of squared distances  
- <img src="https://latex.codecogs.com/gif.latex?x_i"/> is a single data point  
- <img src="https://latex.codecogs.com/gif.latex?\mu_{c_i}"/> is the centroid of the cluster that <img src="https://latex.codecogs.com/gif.latex?x_i"/> belongs to  
- <img src="https://latex.codecogs.com/gif.latex?\|x_i%20-%20\mu_{c_i}\|^2"/> is the squared Euclidean distance between the point and the cluster center  
  
Why is there a norm (|| · ||) in the SSD formula?
  
The Formula uses a **vector norm**, not an absolute value.
  
What is <img src="https://latex.codecogs.com/gif.latex?x_i"/>?
  
- <img src="https://latex.codecogs.com/gif.latex?x_i"/> is a **vector** representing a single data point  
- It can have multiple features (dimensions), for example:  
  - If the data is 2D: <img src="https://latex.codecogs.com/gif.latex?x_i%20=%20[x_1,%20x_2]"/>  
  - If the data is 4D: <img src="https://latex.codecogs.com/gif.latex?x_i%20=%20[x_1,%20x_2,%20x_3,%20x_4]"/>
  
What does <img src="https://latex.codecogs.com/gif.latex?\|x_i%20-%20\mu_{c_i}\|^2"/> mean?
  
- This is the **squared Euclidean distance** between the point <img src="https://latex.codecogs.com/gif.latex?x_i"/> and its cluster center <img src="https://latex.codecogs.com/gif.latex?\mu_{c_i}"/>
- The norm | | means: “calculate the distance in the multi-dimensional space”
- Then we square it to penalize points that are far from the center
  
So it's not an absolute value like in regular math for scalars 
  
**Numerical Example: Squared Distance in SSD**
  
Let's say we have a 2D dataset:
  
- A single data point: <img src="https://latex.codecogs.com/gif.latex?x_i%20=%20[3,%204]"/>  
- Its cluster centroid: <img src="https://latex.codecogs.com/gif.latex?\mu_{c_i}%20=%20[1,%201]"/>
  
We want to calculate:
  
<p align="center"><img src="https://latex.codecogs.com/gif.latex?\|x_i%20-%20\mu_{c_i}\|^2"/></p>  
  
  
Step by step:
  
1. Subtract the centroid from the point:
  
   <p align="center"><img src="https://latex.codecogs.com/gif.latex?x_i%20-%20\mu_{c_i}%20=%20[3%20-%201,\%204%20-%201]%20=%20[2,\%203]"/></p>  
  
  
2. Calculate the squared Euclidean norm:
  
   <p align="center"><img src="https://latex.codecogs.com/gif.latex?\|[2,%203]\|^2%20=%202^2%20+%203^2%20=%204%20+%209%20=%2013"/></p>  
  
  
✅ So the contribution of this data point to the SSD is **13**
  
**What does this mean?**  
The point is “13 units squared” away from the cluster center.  
If many points are far like this, the overall SSD will be large — and the model is less optimal
  
  
—  
it's a vector norm used to calculate distance in **multi-dimensional space**
  
📌 **מה זה אומר בפשטות?**  
לוקחים כל נקודה, מודדים כמה היא רחוקה מהמרכז של הקבוצה שלה, מעלים בריבוע – ואז מחברים את כל הערכים האלה
  
📌 **אם יש כמה קבוצות (קלאסטרים)**  
מחשבים את זה עבור כל קבוצה בנפרד ומחברים הכול כדי לקבל את התמונה הכללית
  
המטרה של המודל היא **להקטין את הערך הזה כמה שיותר**
  
---
  
# Variation Within Cluster
  
כדי למדוד את איכות החלוקה אנחנו משתמשים במרחק הכולל של כל נקודה מהמרכז של הקבוצה שלה  
אם כל הנקודות קרובות מאוד למרכז – סימן שהחלוקה טובה  
ככל שהקבוצות מהודקות – כלומר כל נקודה קרובה מאוד לצנטרואיד – ככה המודל טוב יותר  
כדי למדוד את זה, מחשבים את המרחק של כל נקודה מהמרכז, מעלים בריבוע, ומחברים את כל התוצאות  
המטרה שלנו היא להקטין את הערך הזה כמה שיותר
  
---
  
# Increasing K
  
אם נמשיך להגדיל את מספר הקבוצות – הערך שאנחנו מודדים ילך ויירד  
למה? כי כל נקודה תוכל להיות קרובה יותר לצנטרואיד  
אם נגדיל את קיי למספר מאוד גדול, אפילו כזה שכל נקודה תקבל קבוצה משלה – אז המרחקים יהיו אפס  
אבל זה לא טוב – כי אז אנחנו פשוט מתאימים את עצמנו בדיוק לנתונים הקיימים – וזה נקרא **למידת יתר**  
המודל הזה ייכשל על דאטה חדש
  
<img src="kmean6.png" style="width: 100%" />
  
-
<img src="kmean7.png" style="width: 100%" />
  
---
  
# Elbow Method
  
כדי למצוא את מספר הקבוצות הכי מתאים – אנחנו משתמשים בשיטה שנקראת **שיטת המרפק**  
בונים גרף שבו על ציר X שמים את מספר הקבוצות, ועל ציר Y את מדד המרחקים  
מחפשים את הנקודה שבה השיפור מפסיק להיות משמעותי – שם זה נראה כמו "מרפק"  
משם והלאה, כל תוספת קבוצה לא מוסיפה הרבה לאיכות המודל – אז עוצרים שם
  
<img src="kmean8.png" style="width: 80%" />
  
### WCSS - Within-Cluster Sum of Squares
  
זהו שם נוסף למושג שאנחנו כבר מכירים בשם **סכום ריבועי המרחקים**  
כלומר: בכל קבוצה, מחשבים את המרחק של כל נקודה מהמרכז של הקבוצה שלה, מעלים בריבוע – ואז מחברים את כל זה  
עושים את זה בכל הקבוצות, ומחברים בין כולן  
זה נותן לנו מדד כללי של כמה **הנקודות בכל קלאסטר קרובות למרכז שלהן**
  
📌 **למה זה חשוב?**  
אנחנו רוצים שהמרחקים בתוך כל קבוצה יהיו כמה שיותר קטנים  
ככה נדע שהקלאסטרים טובים והמודל מוצלח
  
📌 **מתי משתמשים בזה?**  
כשרוצים לבחור את מספר הקבוצות האידיאלי בשיטת **המרפק**
  
📉 **הערך הזה תמיד יורד ככל שמעלים את מספר הקבוצות**  
אבל בשלב מסוים הירידה מפסיקה להיות משמעותית – ושם נמצא "המרפק", שזה מספר הקבוצות הכי טוב
  
בנקודה K = 4 רואים שהירידה נעצרת – ולכן זה כנראה המספר האופטימלי לקלאסטרים
  
---
  
# Elbow Method - Python Example
  
## pd.get_dummies
  
הפקודה הזו משמשת כדי להמיר עמודות טקסטואליות לעמודות מספריות בינאריות  
היא יוצרת עמודות חדשות, אחת לכל ערך אפשרי בטקסט, ומסמנת עם 1 או 0 האם הערך מופיע
  
### דוגמה:
  
אם בעמודת 'צבע' יש את הערכים:
- אדום  
- כחול  
- ירוק  
  
אז אחרי הפקודה נקבל שלוש עמודות חדשות:
- צבע_אדום  
- צבע_כחול  
- צבע_ירוק  
  
וכל שורה תקבל 1 בעמודה המתאימה, ו־0 בשאר
  
### למה זה חשוב?
  
מודלים של למידת מכונה לא יודעים להתמודד עם מחרוזות  
לכן חייבים להפוך את המידע הקטגורי למספרים לפני שמכניסים אותו למודל  
המרה כזו נקראת **קידוד אחד חם**, והיא מאפשרת להפעיל חישובים מתמטיים על הדאטה One-Hot Encoding
  
📌 טיפ: לאחר הקידוד כדאי לבצע **סקיילינג** – כדי שכל הערכים יהיו באותו סדר גודל בחישוב המרחקים
  
### דוגמה ל־get_dummies
  
נניח שיש לנו דאטה כזה:
  
| שם     | צבע    | מין   |
|--------|--------|--------|
| דני    | אדום   | זכר   |
| תמר    | כחול   | נקבה  |
| יוסי   | ירוק   | זכר   |
| רוני   | כחול   | נקבה  |
  
אחרי הפעלת הפקודה `get_dummies(df)` נקבל:
  
| שם     | צבע_אדום | צבע_כחול | צבע_ירוק | מין_נקבה | מין_זכר |
|--------|-----------|------------|-----------|------------|-----------|
| דני    | 1         | 0          | 0         | 0          | 1         |
| תמר    | 0         | 1          | 0         | 1          | 0         |
| יוסי   | 0         | 0          | 1         | 0          | 1         |
| רוני   | 0         | 1          | 0         | 1          | 0         |
  
📌 המחשב לא רואה "כחול" או "נקבה" – הוא רואה **0 או 1**  
כך אפשר לחשב מרחקים בין נקודות, כי הכול מבוסס מספרים
  
  
  
הנה דוגמה לפייתון שבה אנחנו מחשבים את הגרף:
  
```python
import pandas as pd
from sklearn.cluster import KMeans
from sklearn.preprocessing import StandardScaler
import matplotlib.pyplot as plt
  
df = pd.read_csv("bank-full.csv")
  
# Convert categorical columns to binary using one-hot encoding
df_dummies = pd.get_dummies(df)
  
# Apply feature scaling
scaler = StandardScaler()
df_scaled = scaler.fit_transform(df_dummies)
  
# Calculate the sum of squared distances (SSD) for K values from 1 to 10
ssd = []
for k in range(1, 11):
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(df_scaled)
    ssd.append(kmeans.inertia_)
  
# Elbow method plot
plt.plot(range(1, 11), ssd, marker='o')
plt.xlabel("Number of Clusters (K)")
plt.ylabel("Within-Cluster Sum of Squares (WCSS)")
plt.title("Elbow Method to Determine Optimal K")
plt.grid(True)
plt.show()
```
  
<img src="kmean9.png" style="width: 80%" />
  