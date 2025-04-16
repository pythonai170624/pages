# הגדרות SVM, Kernel, Kernel Function, ו-Kernel Trick

## Support Vector Machine (SVM)
SVM הוא אלגוריתם למידה מונחית (supervised learning) המשמש לסיווג (classification) ורגרסיה (regression). האלגוריתם מחפש היפרפלן אופטימלי (מישור הפרדה) שיפריד בין קבוצות שונות של נתונים. היפרפלן אופטימלי הוא זה שיוצר את המרווח (margin) המרבי בין הנקודות הקרובות ביותר מכל קבוצה, הידועות כווקטורי תמיכה (support vectors).

מתמטית, עבור נתונים לינאריים, המודל מיוצג על ידי:
- $f(x) = w^T x + b$
- כאשר $w$ הוא וקטור המשקלות
- x הוא וקטור תכונות הקלט
- b הוא ערך ההסטה (bias)
- הנוסחא מתאימה להרבה מימדים ולא רק לדו-מימד

# איך מוצאים את וקטור המשקלים \( w \) ב־SVM

## 🎯 המטרה של SVM
למצוא את הקו (או ההיפר־פליין) שמפריד הכי טוב בין שתי קבוצות, כך שהמרחק מהנקודות הקרובות ביותר מכל צד (ה־**support vectors**) אל הקו יהיה **הכי גדול שאפשר**.

---

## 🔢 איך מוצאים את \( w \)?
המודל מגדיר בעיה מתמטית של **אופטימיזציה** (מציאת מקסימום/מינימום)

### 1. נוסחת ההיפר־פליין:
$$
f(x) = w^T x + b
$$

אם:

$$f(x) \geq 1$$

→ הדגימה שייכת למחלקה החיובית (label = +1)

$$f(x) \leq -1$$ 

→ הדגימה שייכת למחלקה השלילית (label = -1)

---

### 2. תנאי ההפרדה:
לכל דוגמה Xi Yi:

$$
y_i (w^T x_i + b) \geq 1
$$

**מה זה Yi**

ה- Yi זה התווית (label) של הדוגמה ה־ i

כל Xi הוא וקטור

כל Yi הוא מספר שאומר לאיזה קבוצה שייכת הדוגמא, לקבוצה החיובית או לקבוצה השלילית


### 3. פונקציית המטרה (Objective Function):
כדי למקסם את המרחק בין הקבוצות, נמזער את גודל \( w \):

$$
\min \left( \frac{1}{2} \|w\|^2 \right)
$$

תחת ההגבלה:

$$
y_i (w^T x_i + b) \geq 1 \quad \forall i
$$

## ❓ למה צריך להקטין את \( \|w\| \) ב־SVM?


## ✨ ההסבר: כל הסוד נמצא ב־**Margin** – המרווח בין הקבוצות

### המרחק של נקודה מהמישור:
לפי הנוסחה:

$$
\text{Distance from hyperplane} = \frac{|w^T x + b|}{\|w\|}
$$

### המטרה של SVM:
למצוא מישור שמפריד בין הקבוצות עם **המרחק הכי גדול מהנקודות הקרובות ביותר** — כלומר, מרווח (margin) מקסימלי.

### תנאי ההפרדה:

$$
y_i(w^T x_i + b) \geq 1
$$

הנקודות הכי קרובות למישור הן ה־**Support Vectors**, שמקיימות:

$$
y_i(w^T x_i + b) = 1
$$

## ✅ המרחק שלהן מהמישור:

$$
\text{margin} = \frac{1}{\|w\|}
$$


### ולכן:
- ככל ש־\( \|w\| \) **קטן יותר**, המרווח **גדול יותר**.
- כלומר: אם נקטין את \( \|w\| \), אנחנו **מרחיקים** את המישור מהנקודות הקרובות ביותר — וזה בדיוק מה שאנחנו רוצים!

---

## 🧠 ולכן בפונקציית המטרה של SVM:
אנחנו **ממזערים** את:

$$
\frac{1}{2} \|w\|^2
$$


כדי למצוא את ההיפר־פליין עם **margin מקסימלי** ולוודא הפרדה טובה בין הקבוצות.

---

### 🖼️ המחשה בציור:
- מישורים עם \( \|w\| \) גדול מפרידים קרוב מדי לנקודות – רע.  
- מישורים עם \( \|w\| \) קטן יוצרים **רווח גדול** — טוב!



---

## 🤖 איך פותרים את זה בפועל?

1. משתמשים בשיטה מתמטית בשם **Lagrange Multipliers**.
2. פותרים את הבעיה הכפולה (Dual Problem).
3. הפתרון מבוסס רק על ה־**Support Vectors** (הנקודות הכי קרובות להיפר־פליין).
4. מהם מחשבים את \( w \) כך:
$$
w = \sum_i \alpha_i y_i x_i
$$

כאשר:
- \( \alpha_i \) הם פרמטרים שקובעים את חשיבות כל דוגמה.
- רק עבור ה־support vectors יש \( \alpha_i \neq 0 \).

---

## 💡 סיכום
- אנחנו לא מחשבים את \( w \) ישירות, אלא פותרים בעיית אופטימיזציה.
- המטרה היא למצוא את הקו שמפריד הכי טוב בין הקבוצות עם **margin** מקסימלי.
- התוצאה: משוואה שמבוססת רק על ה־support vectors.









---
---
---


## Kernel (גרעין)
קרנל הוא פונקציה מתמטית המאפשרת ל-SVM לטפל בנתונים לא-לינאריים. הוא מאפשר חישוב של מכפלת הנקודות במרחב תכונות גבוה-ממדי מבלי לחשב במפורש את הטרנספורמציה של וקטורי הקלט למרחב זה

## Kernel Function (פונקציית גרעין)
פונקצית הקרנל, המסומנת לרוב כ-

$K(x, y)$

מחשבת את מכפלת הנקודות (dot product) של שני וקטורים x ו-y לאחר שעברו טרנספורמציה למרחב תכונות גבוה יותר, מבלי לחשב במפורש את הטרנספורמציה עצמה:

$K(x, y) = \phi(x) \cdot \phi(y)$

כאשר $\phi$ היא פונקציית הטרנספורמציה למרחב הגבוה יותר

### Common types of Kernel Functions:

1. **Linear Kernel**:
   $K(x, y) = x \cdot y$

2. **Polynomial Kernel**:
   $K(x, y) = (\gamma x \cdot y + c)^d$
   where $\gamma > 0$, $c \geq 0$, and $d$ is an integer representing the degree of the polynomial.

3. **Radial Basis Function (RBF) or Gaussian Kernel**:
   $K(x, y) = \exp(-\gamma \|x - y\|^2)$
   where $\gamma > 0$, typically $\gamma = \frac{1}{2\sigma^2}$.

4. **Sigmoid Kernel**:
   $K(x, y) = \tanh(\gamma x \cdot y + c)$
   where $\gamma > 0$ and $c \geq 0$.

# Kernel Functions with Examples

**What is γ (Gamma)?**

Gamma is a hyperparameter that appears in several kernel functions, including the Polynomial kernel, RBF/Gaussian kernel, and Sigmoid kernel. It controls different aspects of the kernel's behavior:

**The "exp"** in the RBF/Gaussian kernel formula refers to the exponential function, which is commonly written as "exp" in mathematics and programming

The exponential function exp(x) is equivalent to e^x, where "e" is Euler's number (approximately 2.71828...), a mathematical constant that forms the base of natural logarithms

## 1. Linear Kernel
**Formula**: $K(x, y) = x \cdot y$

**Example**:
For two 2D vectors $x = [1, 2]$ and $y = [3, 4]$:

$K(x, y) = x \cdot y = 1 \times 3 + 2 \times 4 = 3 + 8 = 11$

**Use case**: Linear kernels work well when the data is already linearly separable. They're computationally efficient but cannot handle non-linear relationships in data.

## 2. Polynomial Kernel
**Formula**: $K(x, y) = (\gamma x \cdot y + c)^d$

where $\gamma > 0$, $c \geq 0$, and $d$ is the polynomial degree.

**Example**:
For vectors $x = [1, 2]$ and $y = [3, 4]$, with $\gamma = 1$, $c = 1$, and $d = 2$:

$K(x, y) = (1 \times (1 \times 3 + 2 \times 4) + 1)^2 = (11 + 1)^2 = 12^2 = 144$

**Use case**: Polynomial kernels are useful for problems where training data is not linearly separable. The degree $d$ determines the flexibility of the decision boundary. Common choices are $d = 2$ (quadratic) or $d = 3$ (cubic).

## 3. Radial Basis Function (RBF) / Gaussian Kernel
**Formula**: $K(x, y) = \exp(-\gamma \|x - y\|^2)$

where $\gamma > 0$, typically $\gamma = \frac{1}{2\sigma^2}$.

**Example**:
For vectors $x = [1, 2]$ and $y = [3, 4]$ with $\gamma = 0.5$:

1. Calculate the squared Euclidean distance: 
   $\|x - y\|^2 = (1-3)^2 + (2-4)^2 = 4 + 4 = 8$

2. Apply the RBF formula:
   $K(x, y) = \exp(-0.5 \times 8) = \exp(-4) \approx 0.018$

**Use case**: RBF kernels are versatile and work well for most types of data. They're especially effective when the relationship between classes is non-linear. The parameter $\gamma$ controls the "reach" of a single training example's influence.

## 4. Sigmoid Kernel
**Formula**: $K(x, y) = \tanh(\gamma x \cdot y + c)$

tanh = Hyperbolic Tangent 

where $\gamma > 0$ and $c \geq 0$.

**Example**:
For vectors $x = [1, 2]$ and $y = [3, 4]$, with $\gamma = 0.1$ and $c = 0$:

$K(x, y) = \tanh(0.1 \times (1 \times 3 + 2 \times 4)) = \tanh(0.1 \times 11) = \tanh(1.1) \approx 0.8$

**Use case**: The sigmoid kernel comes from neural networks (it's similar to using a neural network with one hidden layer). It's less commonly used in SVMs than RBF kernels but can be effective for specific problems.

## Choosing the Right Kernel

The choice of kernel depends on the specific problem:

- **Linear kernel**: When data is linearly separable
- **Polynomial kernel**: When you need a more flexible decision boundary with clear degree of separation
- **RBF kernel**: Most versatile, works well for most datasets when properly tuned
- **Sigmoid kernel**: Works for specific types of problems, often related to neural networks

In practice, it's common to try different kernels and use cross-validation to determine which one performs best for your specific dataset.

## Visual Intuition

To understand how kernels transform data:

1. **Linear**: Data remains in the same space, separated by a straight line
2. **Polynomial**: Data is mapped to a higher-dimensional space where curved boundaries in original space become linear boundaries
3. **RBF**: Essentially creates a "bump" around each data point, with the width controlled by $\gamma$
4. **Sigmoid**: Creates a decision boundary similar to that of a neural network

The kernel trick allows us to compute these separations without explicitly transforming the data to higher dimensions, making SVMs computationally efficient even for complex decision boundaries.

## Kernel Trick (טריק הגרעין)
ה-Kernel Trick הוא הטכניקה שמאפשרת ל-SVM להתמודד עם בעיות סיווג לא-לינאריות מבלי לחשב במפורש את הטרנספורמציה למרחב גבוה-ממדי. הרעיון הבסיסי הוא:

1. במקום להפעיל טרנספורמציה $\phi$ על כל וקטור קלט $x$ ו-$y$ בנפרד
2. ואז לחשב את מכפלת הנקודות שלהם $\phi(x) \cdot \phi(y)$
3. אנחנו מחשבים ישירות את $K(x, y)$ שנותן את אותה תוצאה

זה חוסך זמן חישוב משמעותי, במיוחד כאשר מרחב התכונות הגבוה-ממדי יכול להיות אינסופי (כמו ב-RBF Kernel).

### היתרונות של Kernel Trick:
- מאפשר ל-SVM להתמודד עם נתונים לא לינאריים
- חוסך בזמן חישוב ובשימוש בזיכרון
- מאפשר עבודה במרחבי תכונות אינסופיים
- משפר את הדיוק בבעיות סיווג מורכבות

### דוגמה פשוטה:
בואו נתאר מקרה של נתונים שלא ניתנים להפרדה לינארית, כמו בעיית XOR (או בעיית המעגל - נקודות בתוך מעגל מול נקודות מחוץ למעגל):

1. במרחב המקורי (הדו-ממדי) אין אפשרות למצוא קו ישר שיפריד בין שתי הקבוצות
2. אם נשתמש בטרנספורמציה כמו $\phi(x,y) = (x, y, x^2 + y^2)$, אנחנו ממפים את הנתונים למרחב תלת-ממדי
3. במרחב התלת-ממדי הזה, ניתן להפריד את הנתונים באמצעות מישור (היפרפלן)
4. במקום לחשב במפורש את הטרנספורמציה הזו, אנחנו יכולים להשתמש ב-Kernel (לדוגמה Gaussian Kernel) שמשיג את אותה תוצאה

ה-Kernel Trick מאפשר לנו לעבוד עם מרחבים בעלי ממדים גבוהים, לעתים אפילו אינסופיים, מבלי לשלם את המחיר החישובי של עבודה במרחבים אלו.

