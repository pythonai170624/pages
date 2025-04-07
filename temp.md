# שיטות איטרטיביות לפתרון משוואות רגרסיה לוגיסטית

## למה נדרשות שיטות איטרטיביות?

המשוואות שקיבלנו מגזירת פונקציית הלוג-נראות הן:

$$\frac{\partial \ln L}{\partial \beta_0} = \sum_{i=1}^{n} [y_i - p_i] = 0$$

$$\frac{\partial \ln L}{\partial \beta_1} = \sum_{i=1}^{n} [x_i(y_i - p_i)] = 0$$

כאשר $p_i = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x_i)}}$

משוואות אלה אינן פתירות באופן אנליטי ישיר (בניגוד לרגרסיה לינארית) מכיוון שהמשתנים $\beta_0$ ו-$\beta_1$ מופיעים בצורה לא-לינארית בתוך פונקציית הסיגמואיד $p_i$. לכן, אנו נדרשים לשיטות איטרטיביות כמו Newton-Raphson או IRLS (Iteratively Reweighted Least Squares).

## Newton-Raphson Method - הסבר עקרוני

שיטת Newton-Raphson היא טכניקה איטרטיבית למציאת שורשים של פונקציות. במקרה של רגרסיה לוגיסטית, אנו מחפשים את השורשים של נגזרות פונקציית הלוג-נראות.

האלגוריתם עובד כך:
1. מתחילים עם ניחוש ראשוני של הפרמטרים $\beta_0$ ו-$\beta_1$
2. מחשבים את הנגזרות ואת מטריצת ההסיאן (מטריצת הנגזרות השניות)
3. מעדכנים את הפרמטרים לפי הנוסחה: $\beta^{(new)} = \beta^{(old)} - H^{-1} \nabla L$
4. חוזרים על שלבים 2-3 עד להתכנסות

## דוגמה מפורטת לאיטרציה אחת

נשתמש בנתונים מהטבלה המקורית ונראה איטרציה אחת של שיטת Newton-Raphson.

### שלב 1: ניחוש התחלתי
נתחיל עם ניחוש שרירותי: $\beta_0 = 0$ ו-$\beta_1 = 0$

### שלב 2: חישוב הסתברויות לפי הפרמטרים הנוכחיים

עם $\beta_0 = 0$ ו-$\beta_1 = 0$, נחשב את $p_i$ לכל תצפית $i$:

$p_i = \frac{1}{1 + e^{-(\beta_0 + \beta_1 x_i)}} = \frac{1}{1 + e^{-(0 + 0 \cdot x_i)}} = \frac{1}{1 + e^0} = \frac{1}{2}$

כלומר, בניחוש ההתחלתי שלנו, ההסתברות לעבור את המבחן היא 50% ללא קשר למספר שעות הלימוד.

### שלב 3: חישוב הנגזרות (Gradient)

נחשב את הנגזרות של פונקציית הלוג-נראות לפי הפרמטרים הנוכחיים:

$$\frac{\partial \ln L}{\partial \beta_0} = \sum_{i=1}^{n} [y_i - p_i]$$

הנתונים שלנו הם:
- עבור $i=1$ עד $i=4$: $y_i = 0$ (נכשל)
- עבור $i=5$ עד $i=9$: $y_i = 1$ (עבר)
- לכל $i$: $p_i = \frac{1}{2}$ (מהניחוש ההתחלתי)

לכן:

$$\frac{\partial \ln L}{\partial \beta_0} = \sum_{i=1}^{9} [y_i - \frac{1}{2}] = (0-\frac{1}{2}) + (0-\frac{1}{2}) + (0-\frac{1}{2}) + (0-\frac{1}{2}) + (1-\frac{1}{2}) + (1-\frac{1}{2}) + (1-\frac{1}{2}) + (1-\frac{1}{2}) + (1-\frac{1}{2})$$

$$= -\frac{1}{2} - \frac{1}{2} - \frac{1}{2} - \frac{1}{2} + \frac{1}{2} + \frac{1}{2} + \frac{1}{2} + \frac{1}{2} + \frac{1}{2} = \frac{1}{2}$$

וכעת עבור $\beta_1$:

$$\frac{\partial \ln L}{\partial \beta_1} = \sum_{i=1}^{n} [x_i(y_i - p_i)]$$

$$= 1 \cdot (0-\frac{1}{2}) + 2 \cdot (0-\frac{1}{2}) + 3 \cdot (0-\frac{1}{2}) + 4 \cdot (0-\frac{1}{2}) + 5 \cdot (1-\frac{1}{2}) + 6 \cdot (1-\frac{1}{2}) + 7 \cdot (1-\frac{1}{2}) + 8 \cdot (1-\frac{1}{2}) + 9 \cdot (1-\frac{1}{2})$$

$$= -\frac{1}{2} - 1 - \frac{3}{2} - 2 + \frac{5}{2} + 3 + \frac{7}{2} + 4 + \frac{9}{2} = 10$$

כלומר, וקטור הגרדיאנט הוא $\nabla L = (\frac{1}{2}, 10)$.

### שלב 4: חישוב מטריצת ההסיאן (Hessian)

מטריצת ההסיאן מכילה את הנגזרות השניות של פונקציית הלוג-נראות:

$$H = 
\begin{pmatrix}
\frac{\partial^2 \ln L}{\partial \beta_0^2} & \frac{\partial^2 \ln L}{\partial \beta_0 \partial \beta_1} \\
\frac{\partial^2 \ln L}{\partial \beta_1 \partial \beta_0} & \frac{\partial^2 \ln L}{\partial \beta_1^2}
\end{pmatrix}$$

עבור רגרסיה לוגיסטית, הנגזרות השניות הן:

$$\frac{\partial^2 \ln L}{\partial \beta_0^2} = -\sum_{i=1}^{n} p_i(1-p_i)$$

$$\frac{\partial^2 \ln L}{\partial \beta_0 \partial \beta_1} = \frac{\partial^2 \ln L}{\partial \beta_1 \partial \beta_0} = -\sum_{i=1}^{n} x_i p_i(1-p_i)$$

$$\frac{\partial^2 \ln L}{\partial \beta_1^2} = -\sum_{i=1}^{n} x_i^2 p_i(1-p_i)$$

במקרה שלנו, $p_i = \frac{1}{2}$ לכל $i$, ולכן $p_i(1-p_i) = \frac{1}{2} \cdot (1-\frac{1}{2}) = \frac{1}{4}$.

נחשב כל אחד מהרכיבים:

$$\frac{\partial^2 \ln L}{\partial \beta_0^2} = -\sum_{i=1}^{9} \frac{1}{4} = -9 \cdot \frac{1}{4} = -\frac{9}{4}$$

$$\frac{\partial^2 \ln L}{\partial \beta_0 \partial \beta_1} = -\sum_{i=1}^{9} x_i \cdot \frac{1}{4} = -\frac{1}{4} \cdot (1+2+3+4+5+6+7+8+9) = -\frac{1}{4} \cdot 45 = -\frac{45}{4}$$

$$\frac{\partial^2 \ln L}{\partial \beta_1^2} = -\sum_{i=1}^{9} x_i^2 \cdot \frac{1}{4} = -\frac{1}{4} \cdot (1^2+2^2+3^2+4^2+5^2+6^2+7^2+8^2+9^2) = -\frac{1}{4} \cdot 285 = -\frac{285}{4}$$

לכן, מטריצת ההסיאן היא:

$$H = 
\begin{pmatrix}
-\frac{9}{4} & -\frac{45}{4} \\
-\frac{45}{4} & -\frac{285}{4}
\end{pmatrix}$$

### שלב 5: עדכון הפרמטרים

כעת נשתמש בנוסחה: $\beta^{(new)} = \beta^{(old)} - H^{-1} \nabla L$

ראשית, נחשב את $H^{-1}$:

$$\det(H) = (-\frac{9}{4}) \cdot (-\frac{285}{4}) - (-\frac{45}{4}) \cdot (-\frac{45}{4}) = \frac{9 \cdot 285}{16} - \frac{45 \cdot 45}{16} = \frac{2565 - 2025}{16} = \frac{540}{16} = \frac{135}{4}$$

$$H^{-1} = \frac{1}{\det(H)}
\begin{pmatrix}
-\frac{285}{4} & \frac{45}{4} \\
\frac{45}{4} & -\frac{9}{4}
\end{pmatrix} = \frac{4}{135}
\begin{pmatrix}
-\frac{285}{4} & \frac{45}{4} \\
\frac{45}{4} & -\frac{9}{4}
\end{pmatrix} =
\begin{pmatrix}
-\frac{285}{135} & \frac{45}{135} \\
\frac{45}{135} & -\frac{9}{135}
\end{pmatrix} =
\begin{pmatrix}
-\frac{19}{9} & \frac{1}{3} \\
\frac{1}{3} & -\frac{1}{15}
\end{pmatrix}$$

כעת נעדכן את הפרמטרים:

$$\begin{pmatrix} \beta_0^{(new)} \\ \beta_1^{(new)} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} - \begin{pmatrix} -\frac{19}{9} & \frac{1}{3} \\ \frac{1}{3} & -\frac{1}{15} \end{pmatrix} \begin{pmatrix} \frac{1}{2} \\ 10 \end{pmatrix}$$

$$= \begin{pmatrix} 0 \\ 0 \end{pmatrix} - \begin{pmatrix} -\frac{19}{9} \cdot \frac{1}{2} + \frac{1}{3} \cdot 10 \\ \frac{1}{3} \cdot \frac{1}{2} - \frac{1}{15} \cdot 10 \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} - \begin{pmatrix} -\frac{19}{18} + \frac{10}{3} \\ \frac{1}{6} - \frac{2}{3} \end{pmatrix}$$

$$= \begin{pmatrix} 0 \\ 0 \end{pmatrix} - \begin{pmatrix} \frac{-19+60}{18} \\ \frac{1-4}{6} \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} - \begin{pmatrix} \frac{41}{18} \\ -\frac{3}{6} \end{pmatrix} = \begin{pmatrix} -\frac{41}{18} \\ \frac{1}{2} \end{pmatrix}$$

לכן, לאחר איטרציה אחת, הפרמטרים המעודכנים הם $\beta_0 \approx -2.28$ ו-$\beta_1 \approx 0.5$.

### שלב 6: המשך האיטרציות

בשלב זה היינו ממשיכים לאיטרציה הבאה עם הפרמטרים החדשים, מחשבים מחדש את ההסתברויות $p_i$ עם הפרמטרים המעודכנים, את הגרדיאנט, את ההסיאן וכן הלאה, עד להתכנסות.

האלגוריתם יתכנס כאשר השינוי בפרמטרים בין איטרציות רצופות הוא קטן מסף מסוים, או כאשר הגרדיאנט קרוב מספיק לאפס.

## שיטות איטרטיביות נוספות

מלבד Newton-Raphson, קיימות שיטות איטרטיביות נוספות:

1. **Gradient Descent**: עדכון הפרמטרים בכיוון הגרדיאנט, אך עם צעדים קטנים יותר:
   $$\beta^{(new)} = \beta^{(old)} - \alpha \nabla L$$
   כאשר $\alpha$ הוא קצב הלמידה.

2. **IRLS (Iteratively Reweighted Least Squares)**: שיטה שממירה את בעיית רגרסיה לוגיסטית לסדרה של בעיות ריבועים פחותים ממושקלים.

3. **BFGS ו-L-BFGS**: שיטות קוואזי-ניוטוניות שמקרבות את ההסיאן במקום לחשב אותו ישירות, מה שמוזיל את העלות החישובית.

## סיכום

ראינו איטרציה אחת של שיטת Newton-Raphson, שהיא אחת השיטות הנפוצות לפתרון משוואות רגרסיה לוגיסטית. האלגוריתם מתחיל מניחוש ראשוני ומשפר את הפרמטרים באופן חוזר ונשנה עד להתכנסות. בדוגמה שלנו, ניחשנו $\beta_0 = 0$ ו-$\beta_1 = 0$, ולאחר איטרציה אחת קיבלנו $\beta_0 \approx -2.28$ ו-$\beta_1 \approx 0.5$. 

איטרציות נוספות יובילו להתכנסות לערכים הסופיים של $\beta_0 \approx -5.6$ ו-$\beta_1 \approx 1.28$ כפי שחושב בדוגמה המלאה.
