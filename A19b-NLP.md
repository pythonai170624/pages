### ניתוח טקסט עם SpaCy – פונקציונליות בסיסית

#### טעינת המודל וניתוח משפט

כדי לנתח טקסט עם **spaCy**, צריך קודם לטעון את המודל לשפה האנגלית – `en_core_web_sm`
המודל מבצע **טוקניזציה** (פירוק של הטקסט למילים או ביטויים) ומחזיר אובייקט מסוג `Doc`, שמכיל את המידע הלשוני על כל מילה

```python
import spacy
nlp = spacy.load("en_core_web_sm")

# The model response is a SpaCy Doc object
doc = nlp("Tesla is looking at buying U.S. startup for $6 million")

for token in doc:
    print(f'[{token.pos:3}]', token.text, token.pos_, token.dep_)
```

Output:
```
[ 96] Tesla PROPN nsubj
[ 87] is AUX aux
[100] looking VERB ROOT
[ 85] at ADP prep
[100] buying VERB pcomp
[ 96] U.S. PROPN dobj
[100] startup VERB advcl
[ 85] for ADP prep
[ 99] $ SYM quantmod
[ 93] 6 NUM compound
[ 93] million NUM pobj
```

הפלט כולל טוקנים עם מאפיינים כמו:

* `text` → הטקסט של המילה
* `pos` part-of-speech tag as an integer → תג חלק הדיבר כמספר (int)
* `pos_`string representation of the POS tag (more human) → סוג המילה (שם עצם, פועל, תואר וכו’) כתיאור מילולי
* `dep_` dependency label → הקשר התחבירי של המילה במשפט (נושא, נשוא, מושא ישיר וכו’)


| טוקן (text) | POS\_ |                | DEP\_    |                         |
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

#### דוגמה: טוקניזציה מתקדמת

* המודל מזהה **U.S.** כטוקן אחד ולא מפצל אותו
* מזהה **\$** כסמל (SYM)
* מזהה **million** כמספר (NUM)

## דוגמא נוספת

```python
import spacy
nlp = spacy.load("en_core_web_sm")

# The model response is a SpaCy Doc object
doc = nlp("Tesla isn't looking into startups anymore")

for token in doc:
    print(f'[{token.pos:3}]', token.text, token.pos_, token.dep_)
```

Output:

```
[ 96] Tesla PROPN nsubj
[ 87] is AUX aux
[ 94] n't PART neg
[100] looking VERB ROOT
[ 85] into ADP prep
[ 92] startups NOUN pobj
[ 86] anymore ADV advmod
```

We can see that the model splitted the 'isn't' word into 2 separated tokens.
First for the 'is' (AUX - auxiliary verbs) Second for 'n't' (neg - negativity word)

### פירוק של מילים מסובכות

המילה **isn't** מתפצלת לשני טוקנים:

* `is` (פועל עזר)
* `n't` (מילת שלילה)

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
