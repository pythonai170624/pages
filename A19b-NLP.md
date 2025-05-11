### ניתוח טקסט עם SpaCy – פונקציונליות בסיסית

#### טעינת המודל וניתוח משפט

כדי לנתח טקסט עם **spaCy**, צריך קודם לטעון את המודל לשפה האנגלית – `en_core_web_sm`
המודל מבצע **טוקניזציה** (פירוק של הטקסט למילים או ביטויים) ומחזיר אובייקט מסוג `Doc`, שמכיל את המידע הלשוני על כל מילה

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Tesla is looking at buying U.S. startup for $6 million")
```

Output:
```
Tesla PROPN nsubj
is AUX aux
looking VERB ROOT
at ADP prep
buying VERB pcomp
U.S. PROPN dobj
startup VERB advcl
for ADP prep
$ SYM quantmod
6 NUM compound
million NUM pobj
```

הפלט כולל טוקנים עם מאפיינים כמו:

* `text` → הטקסט של המילה
* `pos` → תג חלק הדיבר כמספר (int)
* `pos_` → סוג המילה (שם עצם, פועל, תואר וכו’) כתיאור מילולי
* `dep_` → הקשר התחבירי של המילה במשפט (נושא, נשוא, מושא ישיר וכו’)


| טוקן (text) | סוג דקדוקי (pos\_) | תפקיד תחבירי (dep\_) |
| ----------- | ------------------ | -------------------- |
| Tesla       | PROPN              | nsubj                |
| is          | AUX                | aux                  |
| looking     | VERB               | ROOT                 |
| at          | ADP                | prep                 |
| buying      | VERB               | pcomp                |
| U.S.        | PROPN              | dobj                 |
| startup     | VERB               | advcl                |
| for         | ADP                | prep                 |
| \$          | SYM                | quantmod             |
| 6           | NUM                | compound             |
| million     | NUM                | pobj                 |

לדוגמה, אפשר לבדוק את סוג המילה הראשונה:

```python
print(doc[0].pos)   # מחזיר ערך מספרי
print(doc[0].pos_)  # PROPN – שם עצם פרטי
```

#### דוגמה: טוקניזציה מתקדמת

* המודל מזהה **U.S.** כטוקן אחד ולא מפצל אותו
* מזהה **\$** כסמל (SYM)
* מזהה **million** כמספר (NUM)

כאן להוסיף תמונה מהעמוד 14

### פירוק של מילים מסובכות

המילה **isn't** מתפצלת לשני טוקנים:

* `is` (פועל עזר)
* `n't` (מילת שלילה)

כאן להוסיף תמונה מהעמוד 15

### אינדקס וסריקה בלולאות

אפשר לבדוק את `pos_` של מילה לפי אינדקס:

```python
print(doc[0].pos_)  # PROPN
```

אפשר גם לעבור על כל המילים במסמך ולהדפיס את המאפיינים:

```python
for token in doc:
    print(token.text, token.pos, token.pos_, token.dep_)
```

spaCy מאפשר גם יצירת `Span` – קטע מתוך המסמך

### זיהוי משפטים

אפשר לבדוק משפטים במסמך כך:

```python
for sent in doc.sents:
    print(sent.text)
```

spaCy מזהה היכן מתחיל ונגמר כל משפט לפי ההקשר וסימני הפיסוק

כאן להוסיף תמונה מהעמוד 17

### כיצד spaCy מפרק טוקנים בשפה

spaCy משתמש בחוקי טוקניזציה שמתאימים לשפה (למשל אנגלית) שמוגדרים מראש

* **prefixes** – תווים בהתחלה כמו \$, (
* **suffixes** – תווים בסוף כמו ., !
* **infixes** – תווים באמצע כמו קו מפריד ב־"e-mail"

כאן להוסיף תמונה מהעמוד 18
