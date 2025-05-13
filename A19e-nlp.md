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


### שלבים:

1. **Tokenization** – פיצול הטקסט למילים
2. **Feature Extraction** – הפקת מאפיינים כמו צורת מילה, אותיות גדולות, סיומות
3. **Model Training** – אימון מודל על טקסטים מסומנים מראש
4. **Prediction** – זיהוי ישויות בטקסט חדש

`Barack Obama was born on August 4, 1961, in Honolulu, Hawaii.`

#### 1. **Tokenization** – פיצול הטקסט למילים (טוקנים)  
ה- SpaCy מפרקת את המשפט למילים ותווי פיסוק – כמו `"Barack"`, `"Obama"`, `"August"`, `"4"`, `"1961"`, `","`, `"Hawaii"` וכו'

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

```
Barack Obama → PERSON → אדם (אמיתי או בדיוני)
August 4, 1961 → DATE → תאריך
Honolulu → GPE → מדינה / עיר / אזור גיאוגרפי
Hawaii → GPE → מדינה / עיר / אזור גיאוגרפי
```

📌 כך spaCy מצליחה להבין מתוך ההקשר ולסווג נכון כל ישות בטקסט

**פייתון**

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

### 🧠 התאמה אישית של NER ב־spaCy

#### למה נרצה להוסיף ישויות ידנית?
לפעמים spaCy לא מזהה ישויות שאנחנו כן רוצים לזהות – לדוגמה, "Tesla" לא מזוהה כברירת מחדל כישות מסוג ORG  
במקרים כאלה נוכל להוסיף את הישות ידנית למסמך

#### השלבים להוספת ישות ידנית:
1. שליפת הערך המספרי (hash) של התווית הרצויה (למשל "ORG")
2. יצירת Span מהמילה או הביטוי הרצוי
3. הוספת ה־Span לרשימת הישויות במסמך `doc.ents`

```python
import spacy
nlp = spacy.load('en_core_web_sm')

doc = nlp(u'Tesla to build a U.K. factory for $6 million')
for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))  

# adding Tesla entity
from spacy.tokens import Span

ORG = doc.vocab.strings["ORG"]
new_ent = Span(doc, start=0, end=1, label=ORG)
doc.ents = list(doc.ents) + [new_ent]

for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))  
```

Output:
```
-Before
U.K. GPE Countries, cities, states
$6 million MONEY Monetary values, including unit

-After
Tesla ORG Companies, agencies, institutions, etc.
U.K. GPE Countries, cities, states
$6 million MONEY Monetary values, including unit
```

### דוגמה מתקדמת – Phrase Matcher
אם נרצה לזהות ביטויים שלמים כמו:
- `dashboard website`
- `dashboard-website`

In this example, we demonstrate how to use SpaCy’s `PhraseMatcher` to detect custom named entities that are not recognized by default

#### Step 1: Default Behavior – No Entities Detected

```python
doc = nlp(u"Our company plans to introduce a new dashboard-website. If successful, the dashboard website\
 will be our main customer payed product")

for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))  
```

Output:
```
No named entities found.
```

#### Step 2: Define Phrase Matcher Patterns

We want to recognize ```"dashboard website"``` and ```"dashboard-website"``` as **PRODUCT** entities. First, we use PhraseMatcher to define patterns for them

```python
from spacy.matcher import PhraseMatcher

matcher = PhraseMatcher(nlp.vocab)

phrase_list = ['dashboard website', 'dashboard-website']
phrase_patterns = [nlp(text) for text in phrase_list]

matcher.add('newproduct', phrase_patterns)

matches = matcher(doc)
matches
```

Output:
```
[(2689272359382549672, 7, 10), (2689272359382549672, 15, 17)]
```

✅ We created two phrase patterns: one for "dashboard website" and one for "dashboard-website"

✅ Added both patterns under the label 'newproduct'

✅ SpaCy successfully matched both phrases within the doc and returned their span positions

#### Step 3: Add Matched Spans to `doc.ents`

After using the `PhraseMatcher` to find matching phrases in the text, we can now manually add them to SpaCy’s `doc.ents` list so that they are treated as proper named entities.

```python
from spacy.tokens import Span

PROD = doc.vocab.strings['PRODUCT']

new_ents = [Span(doc, match[1], match[2], label=PROD) for match in matches]

doc.ents = list(doc.ents) + new_ents

for ent in doc.ents:
    print(ent.text, ent.label_, spacy.explain(ent.label_))  
```    

Output:
```
dashboard-website PRODUCT Objects, vehicles, foods, etc. (not services)
dashboard website PRODUCT Objects, vehicles, foods, etc. (not services)
```

---

### Sentence Segmentation

### פיצול למשפטים (Sentence Segmentation) 🧩

**פיצול משפטים** – תהליך ב־NLP שמטרתו לחלק טקסט שלם למשפטים נפרדים. המטרה היא לזהות מתי מסתיים משפט אחד ומתחיל משפט חדש

#### 🟣 הגישה הנפוצה

בדרך כלל, פיצול משפטים מתבצע לפי סימני פיסוק כמו:
- נקודה `.`
- סימן שאלה `?`
- סימן קריאה `!`

#### 🟠 מגבלות הגישה הפשוטה

למרות שהשיטה הזו פשוטה, היא עלולה להיות **לא מדויקת** בגלל המורכבות של השפה:

- נקודות מופיעות גם ב**קיצורים** (למשל: `ד"ר`, `ארה"ב`)
- **מספרים עשרוניים** (כמו `3.14`)
- **שלוש נקודות** (`...`)

סימנים אלה לא בהכרח מסמנים סוף משפט.

#### 🟢 פתרונות מתקדמים

מודלים מתקדמים בוחנים גם גורמים נוספים:

- **אותיות רישיות** (האם המילה שאחריה מתחילה באות גדולה)
- **הקשר תחבירי**
- **מילים שפותחות משפטים בדרך כלל**

#### 🧠 למה זה חשוב?

פיצול נכון למשפטים הוא בסיס חשוב למשימות NLP מתקדמות כמו:
- תרגום מכונה
- סיכום טקסטים
- ניתוח סנטימנט

בגלל שפעולות אלו מבוצעות לעיתים קרובות **ברמת משפט**, פיצול לא מדויק יפגע בדיוק התוצאות

כאשר אנו מפצלים טקסט למשפטים בעזרת SpaCy, הפלט נשמר באובייקט בשם `doc.sents`  
זהו לא ממש רשימה, אלא **generator** – אובייקט שמחזיר כל משפט בנפרד בלולאה

```python
import spacy
nlp = spacy.load('en_core_web_sm')

doc = nlp('This is the first sentence. This is another sentence. This is the last sentence.')

for sent in doc.sents:
    print(sent)
```

Output:
```
This is the first sentence.
This is another sentence.
This is the last sentence.
```

כדי להשתמש ביכולות של רשימות (כמו שליפה לפי אינדקס), צריך להמיר את doc.sents לרשימה ידנית:

```python
doc_sents = [sent for sent in doc.sents]

print(doc_sents[0])
```

Output:
```
This is the first sentence.
```

### כלל מותאם לפיצול משפטים לפי תו מותאם אישית (כמו `;`) ✂️

ב־SpaCy ברירת המחדל לפיצול משפטים מתבצעת לפי סימני פיסוק בסיסיים כמו נקודה (`.`).  
אך לפעמים המשפט כולל תווים נוספים שיכולים לסמן סיום משפט – למשל **נקודה־פסיק** (`;`)

#### בעיה:

בדוגמה הבאה, SpaCy לא מפצל את המשפט לשניים, למרות שיש `;` שמפריד בין רעיונות שונים:

```python
doc = nlp('Management is doing things right; leadership is doing the right things. –Peter Drucker')

for sent in doc.sents:
    print(sent)
```

Output:
```
Management is doing things right; leadership is doing the right things.
–Peter Drucker
```

**נגדיר פונקציה שתחפש תו ; ותסמן את הטוקן הבא אחריו כהתחלה של משפט חדש**

הפונקציה תירשם לרצף הפעולות (pipeline) של SpaCy כ־component חדש

```python
from spacy.language import Language

@Language.component('set_custom_boundaries')
def set_custom_boundaries(doc):
    for token in doc[:-1]:
        if token.text == ';':
            doc[token.i + 1].is_sent_start = True
    return doc
```

📌 מה הפונקציה עושה:

עוברת על כל הטוקנים במסמך (חוץ מהאחרון)

אם היא מוצאת `;` היא מסמנת את הטוקן הבא (token.i + 1) כהתחלה של משפט (is_sent_start = True)

הפונקציה מחזירה את ה־doc עם העדכון

**anotaton של Language.component 🧩**

ה־`@Language.component` הוא קישוט (decorator) ב־SpaCy שמאפשר לרשום פונקציה מותאמת אישית כשלב (component) ב־pipeline של SpaCy.  
באמצעותו אנו מודיעים ל־SpaCy שהפונקציה שיצרנו צריכה להיות מופעלת כחלק מתהליך העיבוד של הטקסט.

📌 זה שימושי במיוחד כשאנחנו רוצים להוסיף חוקים מותאמים אישית – כמו כלל לפיצול משפטים לפי `;`  
או לבצע ניתוח מותאם לפני או אחרי שלבים כמו POS או NER.

**SpaCy Pipeline 🛠️**

ה־**Pipeline** של SpaCy הוא רצף של שלבים שמופעלים על הטקסט כדי להמיר אותו מטקסט גולמי למידע שניתן לנתח ולעבוד איתו בתוכנה.

כאשר יוצרים אובייקט מסוג `Doc`, הוא עובר אוטומטית דרך כל שלבי ה־pipeline.

✅ השלבים הנפוצים:
- פ- Tokenization – פיצול הטקסט למילים
- פ- POS Tagging – זיהוי חלקי דיבר
- פ- Dependency Parsing – ניתוח תחבירי
- פ- Lemmatization – הפשטת מילים לשורש
- פ- NER – זיהוי ישויות
- פ- Sentence Segmentation – פיצול למשפטים

**התאמה אישית של ה־Pipeline 🧩**

ניתן לשנות את ה־pipeline כדי להתאים לצרכים שונים:

- ❌ להסיר שלבים שלא צריכים (כמו NER אם לא נדרש)
- ➕ להוסיף שלבים מותאמים משלכם (למשל ניתוח רגשות, סיווג טקסט וכו')

📌 חשוב להבין:
- השלבים מופעלים **בסדר קבוע**
- בכל שלב, האובייקט `Doc` מתעדכן עם המידע שנוצר
- לכן **הסדר שבו השלבים מופעלים חשוב מאוד**

**הוספת רכיב מותאם ל־pipeline של SpaCy 🧱**

לאחר שיצרנו כלל חדש לפיצול משפטים (למשל לפי `;`), אפשר להוסיף אותו כרכיב (component) בתוך ה־pipeline של SpaCy.

לשם כך, עלינו לציין:
- את **שם הרכיב** שיצרנו (`set_custom_boundaries`)
- ואת **המיקום שלו** ב־pipeline (למשל: לפני שלב `parser`)

```python
from spacy.language import Language
import spacy

@Language.component('set_custom_boundaries')
def set_custom_boundaries(doc):
    for token in doc[:-1]:
        if token.text == ';':
            doc[token.i + 1].is_sent_start = True
    return doc

nlp = spacy.load('en_core_web_sm')

nlp.add_pipe('set_custom_boundaries', before='parser')
nlp.pipe_names
```

Output:
```
['tok2vec',
 'tagger',
 'set_custom_boundaries',  # --> new pipeline component
 'parser',
 'attribute_ruler',
 'lemmatizer',
 'ner']
```

ועכשיו נריץ שוב: 

```python
doc = nlp('Management is doing things right; leadership is doing the right things. –Peter Drucker')

for sent in doc.sents:
    print(sent)
```

Output:
```
Management is doing things right;
leadership is doing the right things.
–Peter Drucker
```
