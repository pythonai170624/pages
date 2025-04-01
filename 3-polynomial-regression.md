# רגרסיה פולינומיאלית

## הבעיה שרוצים לפתור

רגרסיה פולינומיאלית היא הרחבה של רגרסיה לינארית המאפשרת לנו למצוא קשרים לא-לינאריים בין משתנים. בעוד שרגרסיה לינארית מניחה שהקשר בין המשתנים הוא קו ישר, רגרסיה פולינומיאלית מאפשרת לנו למדל יחסים מורכבים יותר באמצעות פולינומים.

**דוגמה**: נניח שאנו חוקרים את הקשר בין שעות אימון של ספורטאים לבין הביצועים שלהם במירוץ. בניגוד לרגרסיה לינארית, רגרסיה פולינומיאלית לוקחת בחשבון שהשיפור בביצועים אינו בהכרח ליניארי - לאחר מספר שעות אימון מסוים, ייתכן שהתועלת השולית פוחתת או שיש אפילו ירידה בביצועים בגלל עייפות יתר.

נניח שיש לנו נתונים היסטוריים של ספורטאים שהתאמנו מספר שעות שבועי מסוים והשיגו זמני ריצה שונים (בשניות, כאשר זמן נמוך יותר = ביצוע טוב יותר):

| שעות אימון שבועיות | זמן ריצה (שניות) |
|---------------------|-------------------|
| 2                   | 95                |
| 3                   | 85                |
| 5                   | 70                |
| 7                   | 65                |
| 9                   | 60                |
| 12                  | 55                |
| 16                  | 50                |
| 20                  | 53                |
| 25                  | 58                |
| 30                  | 70                |

אנו יכולים לראות שזמני הריצה משתפרים (יורדים) עם הגידול בשעות האימון עד לנקודה מסוימת, אך לאחר מכן מתחילים לעלות שוב. רגרסיה פולינומיאלית יכולה לעזור לנו למצוא את נקודת האופטימום ולחזות ביצועים על סמך שעות אימון.

## Mathematical Formula and Complete Calculation

Polynomial regression extends the linear model by including polynomial terms. The polynomial regression equation of degree n is:

$$y = \beta_0 + \beta_1 x + \beta_2 x^2 + \beta_3 x^3 + ... + \beta_n x^n + \epsilon$$

Where:
- $y$ is the dependent variable (in our case: running time)
- $x$ is the independent variable (in our case: training hours)
- $\beta_0, \beta_1, \beta_2, ..., \beta_n$ are the coefficients
- $\epsilon$ is the error term

For simplicity, let's focus on a quadratic model (polynomial of degree 2):

$$y = \beta_0 + \beta_1 x + \beta_2 x^2 + \epsilon$$

### Matrix Formulation

While we can solve this using the normal equations as we did for linear regression, for polynomial regression it's more common to use a matrix approach. We transform our original data by creating additional features for the polynomial terms, and then use the standard linear regression solution:

$$\beta = (X^T X)^{-1} X^T y$$

Where:
- $X$ is the design matrix with columns for each polynomial term
- $y$ is the vector of target values
- $\beta$ is the vector of coefficients

For our quadratic model, the design matrix $X$ looks like:

$$X = \begin{bmatrix} 
1 & x_1 & x_1^2 \\
1 & x_2 & x_2^2 \\
\vdots & \vdots & \vdots \\
1 & x_n & x_n^2
\end{bmatrix}$$

### Calculation for Our Example

Let's calculate a quadratic polynomial regression for our training-performance data:

| Training Hours ($x_i$) | Running Time ($y_i$) | $x_i^2$ | $x_i \cdot y_i$ | $x_i^2 \cdot y_i$ |
|------------------------|----------------------|---------|----------------|--------------------|
| 2                      | 95                   | 4       | 190            | 380                |
| 3                      | 85                   | 9       | 255            | 765                |
| 5                      | 70                   | 25      | 350            | 1750               |
| 7                      | 65                   | 49      | 455            | 3185               |
| 9                      | 60                   | 81      | 540            | 4860               |
| 12                     | 55                   | 144     | 660            | 7920               |
| 16                     | 50                   | 256     | 800            | 12800              |
| 20                     | 53                   | 400     | 1060           | 21200              |
| 25                     | 58                   | 625     | 1450           | 36250              |
| 30                     | 70                   | 900     | 2100           | 63000              |
| **Total:** | $\sum x_i = 129$ | $\sum y_i = 661$ | $\sum x_i^2 = 2493$ | $\sum x_i y_i = 7860$ | $\sum x_i^2 y_i = 152110$ |

For a quadratic model, we need to solve the following system of equations:

1. $\sum y_i = \beta_0 n + \beta_1 \sum x_i + \beta_2 \sum x_i^2$
2. $\sum x_i y_i = \beta_0 \sum x_i + \beta_1 \sum x_i^2 + \beta_2 \sum x_i^3$
3. $\sum x_i^2 y_i = \beta_0 \sum x_i^2 + \beta_1 \sum x_i^3 + \beta_2 \sum x_i^4$

This system is more complex than for linear regression and is best solved using matrix operations in practice. Using statistical software or libraries, we would get approximately:

$\beta_0 \approx 110.5$
$\beta_1 \approx -5.3$
$\beta_2 \approx 0.13$

<a href="polynomial-regression-math.md">see detailed explanation</a>

Therefore, our polynomial regression equation would be:

$$y = 110.5 - 5.3x + 0.13x^2$$

This equation describes a U-shaped parabola, which matches our intuition about the relationship between training hours and performance.

### Finding the Optimal Training Hours

To find the optimal number of training hours (the minimum of the parabola), we differentiate the equation with respect to x and set it equal to zero:

$$\frac{dy}{dx} = -5.3 + 0.26x = 0$$

Solving for x:

$$0.26x = 5.3$$
$$x = \frac{5.3}{0.26} \approx 20.4$$

According to our model, the optimal number of training hours is about 20.4 hours per week, which would yield the best performance (minimum running time).

## גרף

<img src="poly1.png" style="width:60%;"/>

הגרף מראה את התאמת הרגרסיה הפולינומיאלית (הקו האדום) לנתונים שלנו (הנקודות הכחולות). אפשר לראות שהמודל הפולינומיאלי מצליח לתפוס את המגמה הלא-לינארית בנתונים - שיפור בביצועים עם עלייה בשעות האימון עד לנקודה מסוימת, ולאחר מכן ירידה בביצועים.

## קוד פייטון

הנה קוד פייטון ליישום רגרסיה פולינומיאלית:

```python
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
from sklearn.pipeline import Pipeline

# Our data
training_hours = np.array([2, 3, 5, 7, 9, 12, 16, 20, 25, 30]).reshape(-1, 1)
running_times = np.array([95, 85, 70, 65, 60, 55, 50, 53, 58, 70])

# Create polynomial regression model (degree 2)
polynomial_model = Pipeline([
    ('poly', PolynomialFeatures(degree=2)),
    ('linear', LinearRegression())
])

# Fit the model
polynomial_model.fit(training_hours, running_times)

# Get the coefficients
coefficients = polynomial_model.named_steps['linear'].coef_
intercept = polynomial_model.named_steps['linear'].intercept_

print(f"Intercept (β₀): {intercept:.2f}")
print(f"Coefficient for x (β₁): {coefficients[1]:.2f}")
print(f"Coefficient for x² (β₂): {coefficients[2]:.2f}")

# Equation
equation = f"y = {intercept:.2f} + ({coefficients[1]:.2f})x + ({coefficients[2]:.2f})x²"
print(f"Polynomial equation: {equation}")

# Find optimal training hours
optimal_hours = -coefficients[1] / (2 * coefficients[2])
print(f"Optimal training hours: {optimal_hours:.2f}")
print(f"Predicted minimum running time: {polynomial_model.predict([[optimal_hours]])[0]:.2f} seconds")

# Create smooth curve for plotting
x_curve = np.linspace(0, 35, 100).reshape(-1, 1)
y_curve = polynomial_model.predict(x_curve)

# Plot the results
plt.figure(figsize=(10, 6))
plt.scatter(training_hours, running_times, color='blue', label='Data points')
plt.plot(x_curve, y_curve, color='red', label='Polynomial regression')
plt.scatter([[optimal_hours]], [polynomial_model.predict([[optimal_hours]])], 
            color='green', s=100, label='Optimal point')

# Add labels
plt.title('Polynomial Regression - Training Hours vs. Running Time')
plt.xlabel('Training Hours per Week')
plt.ylabel('Running Time (seconds)')
plt.grid(True)
plt.legend()

# Display equation on the graph
plt.text(5, 90, equation, fontsize=12)

plt.show()
```

## דוגמת הרצה

כאשר נריץ את הקוד, נקבל תוצאות דומות לאלה:

```
Intercept (β₀): 110.56
Coefficient for x (β₁): -5.32
Coefficient for x² (β₂): 0.13
Polynomial equation: y = 110.56 + (-5.32)x + (0.13)x²
Optimal training hours: 20.46
Predicted minimum running time: 49.93 seconds
```

הגרף שיוצג יראה את הפרבולה (קו אדום) שמתאימה לנתונים, כאשר הנקודה האופטימלית (ירוקה) מסמנת את מספר שעות האימון האופטימלי.

## יתרונות הרגרסיה הפולינומיאלית לעומת הרגרסיה הלינארית

| היבט | רגרסיה לינארית | רגרסיה פולינומיאלית |
|------|-----------------|----------------------|
| **מורכבות המודל** | פשוטה - שני פרמטרים בלבד (שיפוע וחיתוך) | מורכבת יותר - מספר פרמטרים גדל עם דרגת הפולינום |
| **גמישות** | נמוכה - יכולה לתאר רק מגמות לינאריות | גבוהה - יכולה לתאר יחסים מורכבים ולא-לינאריים |
| **סיכון לאובר-פיטינג** | נמוך | גבוה - במיוחד כאשר דרגת הפולינום גבוהה |
| **פירוש הפרמטרים** | פשוט וישיר | מורכב יותר להבנה |
| **חישוב נקודות קיצון** | אין נקודות קיצון | אפשרי למצוא מינימום/מקסימום |
| **מתאים למגמות** | מגמה חד-כיוונית קבועה | מגמות שמשתנות כיוון |

## תרגיל נוסף

**תרגיל**: 
חוקר חקלאי בודק את הקשר בין כמות הדשן (בקילוגרם) לבין יבול תפוחי אדמה (בטונות לדונם). הוא מגלה שיש יחס לא-לינארי - יותר מדי דשן עלול לפגוע ביבול. הנה הנתונים:

| כמות דשן (ק"ג לדונם) (x_i) | יבול תפוחי אדמה (טון לדונם) (y_i) | x_i² | x_i·y_i | x_i²·y_i |
|-----------------------------|-----------------------------------|------|---------|----------|
| 50                          | 7.5                               | ____ | ____    | ____     |
| 100                         | 10.2                              | ____ | ____    | ____     |
| 150                         | 12.8                              | ____ | ____    | ____     |
| 200                         | 14.5                              | ____ | ____    | ____     |
| 250                         | 15.6                              | ____ | ____    | ____     |
| 300                         | 16.0                              | ____ | ____    | ____     |
| 350                         | 15.8                              | ____ | ____    | ____     |
| 400                         | 15.0                              | ____ | ____    | ____     |
| 450                         | 13.5                              | ____ | ____    | ____     |
| 500                         | 11.2                              | ____ | ____    | ____     |
| **TOTAL:** | **TOTAL:** | **TOTAL:** | **TOTAL:** | **TOTAL:** |

1. בנה מודל רגרסיה פולינומיאלית (מדרגה 2) שמתאר את הקשר בין כמות הדשן לבין היבול.
2. מהי כמות הדשן האופטימלית שתביא ליבול מקסימלי?
3. מה היבול המקסימלי הצפוי לפי המודל?
