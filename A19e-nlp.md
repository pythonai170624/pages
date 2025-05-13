# NLP FETAURES

## מהו קורפוס (Corpus)

קורפוס הוא אוסף גדול של טקסטים המשמש ללימוד וניתוח שפה טבעית. זה יכול להיות טקסטים ספרותיים, חדשות, תמלולים של דיבור, אתרי אינטרנט ועוד

בקונטקסט של NLP, קורפוס משמש לאימון, בדיקה והערכה של מודלים – לדוגמה, מודלים של חלקי דיבר (POS) או זיהוי ישויות (NER)

#### סוגי קורפוסים:

* **קורפוס לא מסומן** (raw corpus): טקסט רגיל ללא תיוג
* **קורפוס מסומן** (annotated corpus): כולל מידע נוסף כמו חלקי דיבר, תחביר, ישויות וכו'

📌 ה- SpaCy אומנה על קורפוסים מסומנים, מה שמאפשר לה להבין הקשרים ולזהות מבנים לשוניים מורכבים

**דוגמא 1**

```python
import spacy
nlp = spacy.load('en_core_web_sm')
doc = nlp("The quick brown fox jumped over the lazy dog's back.")

print(doc[4].text, doc[4].pos_, doc[4].tag_, spacy.explain(doc[4].tag_))
```

Output:
```
jumped VERB VBD verb, past tense
```

SpaCy decided that the token jumped is a verb with the VBD tag, meaning it's a verb in past tense

**דוגמא 2**

```python
doc1 = nlp(u'I read books on NLP.')
doc2 = nlp(u'I read a book on NLP.')

print(f'{doc1[1].text:10} {doc1[1].pos_:8} {doc1[1].tag_:6} {spacy.explain(doc1[1].tag_)}')
print()
print(f'{doc2[1].text:10} {doc2[1].pos_:8} {doc2[1].tag_:6} {spacy.explain(doc2[1].tag_)}')
```

Output:
```
read       VERB     VBP    verb, non-3rd person singular present

read       VERB     VBD    verb, past tense
```

📌 SpaCy is smart enough to understand from the textual context that the first read token is in present tense, while the second read is in past tense

## Named Entity Recognition (NER)

ה- NER הוא תהליך שבו מזהים ישויות בשם בטקסט (כמו שמות של אנשים, מקומות, חברות, סכומים, תאריכים וכו')

#### • **Named Entities**  
בניית רשימה של ישויות בשם מתוך הטקסט, כשהמפתח הוא סוג הישות והערכים הם המופעים שזוהו  
לדוגמה:
- **Persons (PER)** – שמות של אנשים כמו: `"Albert Einstein"`, `"Marie Curie"`
- **Locations (LOC)** – מקומות גיאוגרפיים כמו: `"Paris"`, `"Mount Everest"`

#### • **Recognition**  
זיהוי בפועל של המילים או הביטויים בטקסט שמייצגים ישויות בשם  
לדוגמה: במשפט `"Barack Obama visited Paris"` – נזהה את `"Barack Obama"` ואת `"Paris"` כישויות

#### • **Classification**  
שיוך כל ישות שנמצאה לקטגוריה מתאימה (כמו `PERSON`, `LOCATION`, `ORG`, `DATE` וכו')


#### שלבים:

1. **Tokenization** – פיצול הטקסט למילים
2. **Feature Extraction** – הפקת מאפיינים כמו צורת מילה, אותיות גדולות, סיומות
3. **Model Training** – אימון מודל על טקסטים מסומנים מראש
4. **Prediction** – זיהוי ישויות בטקסט חדש

ונבצע עליו תהליך זיהוי ישויות בשם (NER). התהליך כולל ארבעה שלבים עיקריים:

`Barack Obama was born on August 4, 1961, in Honolulu, Hawaii.`

#### 1. **Tokenization** – פיצול הטקסט למילים (טוקנים)  
SpaCy מפרקת את המשפט למילים ותווי פיסוק – כמו `"Barack"`, `"Obama"`, `"August"`, `"4"`, `"1961"`, `","`, `"Hawaii"` וכו'

#### 2. **Feature Extraction** – הפקת מאפיינים מהטוקנים  
לכל מילה מחולצים מאפיינים כמו:  
- האם האות הראשונה גדולה? (Barack)  
- האם זו מילה מספרית או תאריך? (4, 1961)  
- האם היא ממוקמת אחרי מילת מפתח כמו "born on"?  

מאפיינים אלו עוזרים למודל להבין אילו מילים עשויות להיות ישויות

#### 3. **Model Training** – (כבר בוצע מראש על ידי spaCy)  
המודל שאומן על קורפוס גדול יודע לזהות מבנים של שמות פרטיים, תאריכים, ערים ועוד

#### 4. **Prediction** – חיזוי הישויות בטקסט  
המודל מזהה את הישויות הבאות:



```python
doc = nlp("Barack Obama was born on August 4, 1961, in Honolulu, Hawaii.")
for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))
```

Output:
```
Barack Obama PERSON People, including fictional
August 4, 1961 DATE Absolute or relative dates or periods
Honolulu GPE Countries, cities, states
Hawaii GPE Countries, cities, states
```

📌 ה- SpaCy מחזיר ישויות עם label כמו `PERSON`, `DATE`, `GPE` ומאפשר להסביר כל label באמצעות `spacy.explain`

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
