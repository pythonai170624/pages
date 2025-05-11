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
doc2 = nlp("Tesla isn't looking into startups anymore.")

for token in doc2:
    print(token.text, token.pos_, token.dep_)
```

| טוקן (text) | POS\_ | הסבר POS\_  | DEP\_  | הסבר DEP\_               |
| ----------- | ----- | ----------- | ------ | ------------------------ |
| Tesla       | PROPN | שם עצם פרטי | nsubj  | נושא המשפט               |
| is          | AUX   | פועל עזר    | aux    | פועל עזר של הפועל המרכזי |
| n't         | PART  | מילת שלילה  | neg    | שלילה שמתווספת לפועל     |
| looking     | VERB  | פועל        | ROOT   | הפועל המרכזי במשפט       |
| into        | ADP   | מילת יחס    | prep   | מילת יחס שמובילה למושא   |
| startups    | NOUN  | שם עצם      | pobj   | מושא של מילת היחס "into" |
| anymore     | ADV   | תואר פועל   | advmod | תיאור שמתווסף לפועל      |
| .           | PUNCT | סימן פיסוק  | punct  | סימן שמסיים משפט         |

```python
for token in doc:
    print(token.text, token.pos_, token.dep_)
```

| טוקן (text) | POS\_ | הסבר POS\_                | DEP\_    | הסבר DEP\_                        |
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

במקרה של מילים מקוצרות (contractions), spaCy מזהה את חלקי המילה המופרדים ומתייחס אליהם כטוקנים נפרדים

למשל:

```python
doc = nlp("Tesla isn't looking into startups anymore.")
for token in doc:
    print(token.text, token.pos_, token.dep_)
```

המודל מחלק את "isn't" ל־`is` (פועל עזר מסוג AUX) ול־`n't` (מילת שלילה מסוג PART)
זה מאפשר להבין בצורה מדויקת יותר את משמעות המשפט ואת התפקיד התחבירי של כל חלק

spaCy מזהה גם את "Tesla" ו־"startups" כ־PROPN (שם עצם פרטי) ו־NOUN (שם עצם רגיל) בהתאמה

כאן להוסיף תמונה מהעמוד 15

המילה **isn't** מתפצלת לשני טוקנים:

* `is` (פועל עזר)
* `n't` (מילת שלילה)

כאן להוסיף תמונה מהעמוד 15

### אינדקס וסריקה בלולאות

אם אנחנו רוצים להבין מה המשמעות של תג מסוים שמחזירה לנו spaCy, אפשר להשתמש בפונקציה `spacy.explain()` שמספקת הסבר טקסטואלי ברור לכל תג תחבירי או דקדוקי

לדוגמה:

```python
import spacy
import spacy.explain

print(spacy.explain("AUX"))    # Auxiliary verb
print(spacy.explain("nsubj"))  # Nominal subject
print(spacy.explain("ROOT"))   # Root of the sentence
```

הפלט עוזר לנו להבין במה מדובר מבלי לנחש את הפירוש של ראשי התיבות

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
