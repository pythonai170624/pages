# NLP FETAURES

## מהו קורפוס (Corpus)

קורפוס הוא אוסף גדול של טקסטים המשמש ללימוד וניתוח שפה טבעית. זה יכול להיות טקסטים ספרותיים, חדשות, תמלולים של דיבור, אתרי אינטרנט ועוד

בקונטקסט של NLP, קורפוס משמש לאימון, בדיקה והערכה של מודלים – לדוגמה, מודלים של חלקי דיבר (POS) או זיהוי ישויות (NER)

#### סוגי קורפוסים:

* **קורפוס לא מסומן** (raw corpus): טקסט רגיל ללא תיוג
* **קורפוס מסומן** (annotated corpus): כולל מידע נוסף כמו חלקי דיבר, תחביר, ישויות וכו'

📌 ה- SpaCy אומנה על קורפוסים מסומנים, מה שמאפשר לה להבין הקשרים ולזהות מבנים לשוניים מורכבים

## Named Entity Recognition (NER)

NER הוא תהליך שבו מזהים ישויות בשם בטקסט (כמו שמות של אנשים, מקומות, חברות, סכומים, תאריכים וכו').

#### שלבים:

1. **Tokenization** – פיצול הטקסט למילים
2. **Feature Extraction** – הפקת מאפיינים כמו צורת מילה, אותיות גדולות, סיומות
3. **Model Training** – אימון מודל על טקסטים מסומנים מראש
4. **Prediction** – זיהוי ישויות בטקסט חדש

```python
doc = nlp("Barack Obama was born on August 4, 1961, in Honolulu, Hawaii.")
for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))
```

📌 SpaCy מחזיר ישויות עם label כמו `PERSON`, `DATE`, `GPE` ומאפשר להסביר כל label באמצעות `spacy.explain()`

---

### NER Customization – התאמה אישית של ישויות

ניתן להוסיף ישויות שלא זוהו ע"י SpaCy כברירת מחדל. לדוגמה: להגדיר את 'Tesla' כארגון (`ORG`), אם SpaCy לא מזהה זאת לבד.

```python
from spacy.tokens import Span
ORG = doc.vocab.strings["ORG"]
new_ent = Span(doc, start=0, end=1, label=ORG)
doc.ents = list(doc.ents) + [new_ent]
```

אפשר גם להשתמש ב־PhraseMatcher כדי לזהות תבניות קבועות כמו:

* `dashboard-website`
* `dashboard website`
  ולהגדיר אותן כ־`PRODUCT`

לאחר הזיהוי עם phrase matcher – מוסיפים את ה־Span לרשימת הישויות `doc.ents`

---

### Sentence Segmentation

פיצול הטקסט למשפטים הוא שלב חשוב לפני ניתוח טקסט. SpaCy משתמשת בסימני פיסוק (כמו `.` `!` `?`) כדי לקבוע היכן מסתיים משפט.

```python
for sent in doc.sents:
    print(sent)
```

📌 ניתן לגשת אל כל המשפטים באמצעות `doc.sents`. אך זו אינה רשימה, אלא generator, לכן נוכל להפוך אותה לרשימה עם `list(doc.sents)`

---

### Custom Segmentation Rules

לפעמים נרצה ש־SpaCy תפריד גם לפי סימנים אחרים (כמו `;`). נוכל להוסיף חוק מותאם:

```python
from spacy.language import Language

@Language.component("set_custom_boundaries")
def set_custom_boundaries(doc):
    for i, token in enumerate(doc[:-1]):
        if token.text == ";":
            doc[i + 1].is_sent_start = True
    return doc

nlp.add_pipe("set_custom_boundaries", before="parser")
```

📌 פונקציית החוק מזהה תו כמו `;` ומסמנת שהטוקן הבא אחריו הוא התחלה של משפט

---

### SpaCy Pipeline

Pipeline הוא רצף הפעולות ש־SpaCy מבצעת על הטקסט:

1. Tokenization
2. POS Tagging
3. Dependency Parsing
4. Lemmatization
5. Named Entity Recognition
6. Sentence Segmentation

כל שלב משפיע על ה־`Doc` ומוסיף לו מידע. אפשר גם להוסיף שלבים מותאמים כמו segmentation או זיהוי ישויות מותאמות

📌 ניתן להוסיף שלב עם `add_pipe()` או להסיר עם `disable()`
