# Naive Bias in NLP

אנחנו רוצים לסווג טקסטים – למשל:
- האם ביקורת לסרט היא חיובית או שלילית?
- האם מייל הוא ספאם או לא?

### מה הקשר לנוסחת בייס?

כמו שלמדנו קודם:

$$
P(A|B) = \frac{P(B|A) \cdot P(A)}{P(B)}
$$

עכשיו נשתמש בזה כדי לסווג טקסטים לפי מילים (tokens)

### איך זה נראה ב־NLP?

#### עבור מילה אחת

עבור מילה אחת אנחנו רוצים לחשב:

מה ההסתברות שהמילה שייכת לקטגוריה מסוימת (למשל – חיובי)?

$$
P(C_k|x) = \frac{P(C_k) \cdot P(x|C_k)}{P(x)}
$$

- **P(Ck | x)**: The probability that the text (x) belongs to class Ck  
  → This is what we want to find

- **P(Ck)**: The prior probability of class Ck  
  → How common this class is in the **training data** (e.g., how many of the training reviews are labeled as positive)

- **P(x | Ck)**: The likelihood  
  → How likely it is to see the words x in texts that belong to class Ck. This is calculated from the **training data**

- **P(x)**: The evidence or total probability of x  
  → How likely the words x are in general, across all classes (from the **training data**)

#### עבור אוסף של מילים

נסמן את הטקסט שלנו כקבוצה של מילים:

$$
\mathbf{x} = (x_1, x_2, ..., x_n)
$$

כלומר: טקסט הוא פשוט אוסף של מילים כמו "good", "movie", "thrilling", וכו'

אנחנו רוצים לחשב:
מה ההסתברות שהטקסט שייך לקטגוריה מסוימת (למשל – חיובי), לפי המילים שבתוכו?

**כאשר נציב את וקטור המילים ונערוך מספר פעולות מתמטיות נקבל:**

$$
P(C_k | \mathbf{x}) \propto P(C_k) \cdot \prod_{i=1}^{n} P(x_i | C_k)
$$

אל תיבהל! בוא נפרק:

- **P(Ck | x)**: The probability that the text belongs to category Ck (e.g., positive)
- **P(Ck)**: How often this category appeared in the past (e.g., how many past texts were positive)
- **P(Xi | Ck)**: How often the word Xi appeared in texts of category Ck
- **prod**: Product symbol – multiply all the word probabilities one after the other


הסימן הזה ( \propto \) אומר: "פרופורציונלי ל־".  
כלומר: זה לא הנוסחה המלאה עם המונה והמחנה – רק החלק החשוב שמשווה בין הקטגוריות

## מה עושים בפועל?

1. סופרים כמה טקסטים יש מכל קטגוריה → זה \( P(C_k) \)
2. סופרים כמה פעמים כל מילה מופיעה בכל קטגוריה → זה \( P(x_i | C_k) \)
3. מחלקים טקסט חדש למילים, מחשבים את ההסתברויות האלה עבור כל קטגוריה
4. בוחרים את הקטגוריה עם **ההסתברות הכי גבוהה**

## דוגמה פשוטה (בלי מתמטיקה):

- יש לנו טקסט: `"great movie"`
- המודל יודע שמילה "great" מופיעה רק בטקסטים חיוביים
- המודל מחשב שההסתברות שזה חיובי גבוהה יותר מהאפשרות שזה שלילי
- לכן הוא יסווג את הטקסט כ־**חיובי**

