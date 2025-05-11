### מה זה `ents` ב־spaCy?

התכונה `ents` של האובייקט `Doc` מכילה את כל **הישויות בשם** (Named Entities) ש־spaCy זיהתה בטקסט

ישויות בשם הן מילים או צירופים שמתארים דברים ממשיים בעולם, כמו:

* שמות של **אנשים** (PERSON)
* **חברות וארגונים** (ORG)
* **מדינות וערים** (GPE)
* **תאריכים** (DATE)
* **סכומים כספיים** (MONEY)

### דוגמה בסיסית:

```python
import spacy
nlp = spacy.load("en_core_web_sm")

doc = nlp("Apple is planning to invest $10 million in Israel in 2024")

for ent in doc.ents:
    print(ent.text, ent.label_)
```

#### פלט:

```
Apple ORG
$10 million MONEY
Israel GPE
2024 DATE
```

### תכונות של כל ישות:

* `ent.text` → הטקסט של הישות
* `ent.label_` → סוג הישות (כמו ORG, GPE)
* `spacy.explain(ent.label_)` → הסבר טקסטואלי לקטגוריה

### דוגמה עם הסברים:

```python
for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))
```

#### פלט:

```
Apple ORG Companies, agencies, institutions, etc.
$10 million MONEY Monetary values, including unit
Israel GPE Countries, cities, states
2024 DATE Absolute or relative dates or periods
```

### סיכום:

* `ents` היא רשימת הישויות ש־spaCy זיהתה במסמך
* כל ישות כוללת טקסט, סוג, והסבר על הסוג
* ניתן לעבור על הרשימה בלולאה ולעבד אותה כרצונך
