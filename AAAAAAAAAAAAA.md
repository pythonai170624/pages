### ניתוח טקסט עם SpaCy – פונקציונליות בסיסית

#### טעינת המודל וניתוח משפט

כדי לנתח טקסט עם **spaCy**, צריך קודם לטעון את המודל לשפה האנגלית – `en_core_web_sm`
המודל מבצע **טוקניזציה** (פירוק של הטקסט למילים או ביטויים) ומחזיר אובייקט מסוג `Doc`, שמכיל את המידע הלשוני על כל מילה

```python
import spacy

nlp = spacy.load("en_core_web_sm")
doc = nlp("Tesla is looking at buying U.S. startup for $6 million")
```

הפלט כולל טוקנים עם מאפיינים כמו:

* `text` → הטקסט של המילה
* `pos` → תג חלק הדיבר כמספר (int)
* `pos_` → סוג המילה (שם עצם, פועל, תואר וכו’) כתיאור מילולי
* `dep_` → הקשר תחבירי של המילה במשפט (נושא, נשוא, מושא ישיר וכו’)

לדוגמה, אפשר לבדוק את סוג המילה הראשונה:

```python
print(doc[0].pos)   # מחזיר ערך מספרי
print(doc[0].pos_)  # PROPN – שם עצם פרטי
```

#### דוגמה: טוקניזציה מתקדמת

```python
for token in doc:
    print(token.text, token.pos_, token.dep_)
```

| טוקן (text) | POS\_ |  POS\_                | DEP\_    |  DEP\_                        |
| ----------- | ----- | ------------------------- | -------- | --------------------------------- |
| Tesla       | PROPN | שם עצם פרטי (Proper Noun) | nsubj    | נושא המשפט                        |
| is          | AUX   | פועל עזר (Auxiliary Verb) | aux      | פועל עזר של הפועל המרכזי          |
| looking     | VERB  | פועל (Verb)               | ROOT     | הפועל המרכזי במשפט                |
| at          | ADP   | מילת יחס (Adposition)     | prep     | מילת יחס שמקשרת לפועל             |
| buying      | VERB  | פועל (Verb)               | pcomp    | משלים של הפועל באמצעות מילת יחס   |
| U.S.        | PROPN | שם עצם פרטי               | dobj     | מושא ישיר של הפועל                |
| startup     | VERB  | פועל                      | advcl    | פסוקית נסיבתית שמוסיפה מידע לפועל |
| for         | ADP   | מילת יחס                  | prep     | מילת יחס שמובילה למושא נוסף       |
| \$          | SYM   | סימן (Symbol)             | quantmod | תיאור כמות שמקדים מספר            |
| 6           | NUM   | מספר (Numeral)            | compound | תיאור מקדים של שם עצם אחר         |
| million     | NUM   | מספר (Numeral)            | pobj     | מושא של מילת היחס "for"           |

ניתן להשתמש ב־`spacy.explain("TAG")` כדי לקבל הסבר לכל תג:

```python
import spacy.explain
print(spacy.explain("aux"))     # auxiliary
print(spacy.explain("ROOT"))    # root of the sentence
```

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
* \*\*inf
