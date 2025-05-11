# ניתוח טקסט עם SpaCy – פונקציונליות בסיסית המשך

## ה- Lemmatization – הפשטת מילים לצורת בסיס תקנית

ה- Lemmatization היא שיטה לעיבוד מילים בשפה טבעית שבה כל מילה מומרת לצורת הבסיס שלה (lemma) לפי ההקשר התחבירי שלה

בניגוד ל־stemming שפשוט חותך סיומות לפי חוקים, lemmatization מתחשבת במשמעות הדקדוקית של המילה, בחלק הדיבר שלה, ובמבנה הלשוני הכללי

לדוגמה:

* המילה `running` תהפוך ל־`run`
* המילה `better` תהפוך ל־`good`

### יתרונות lemmatization

* מבוססת על ניתוח תחבירי ולא על חיתוך טכני
* מחזירה מילים תקניות שקיימות במילון
* מדויקת יותר מהבחינה הסמנטית

### דוגמה:

```python
import spacy
nlp = spacy.load("en_core_web_sm")
doc = nlp("run running ran")
for token in doc:
    print(token.text, "→", token.lemma_)
```

פלט:

```
run → run
running → run
ran → run
```

### ה- Stop Words – מילות עצירה

מילות עצירה הן מילים נפוצות בשפה שאין להן תרומה משמעותית להבנת התוכן המרכזי בטקסט
דוגמאות: the, is, and, in, to, etc

מילים אלו מוסרות בדרך כלל לפני ניתוח טקסט כמו ניתוח רגשות או חיפוש מסמכים

#### למה מסירים אותן

* מפחיתות את מספר הטוקנים
* מחדדות את התוכן המרכזי
* מייעלות את תהליך הלמידה או הניתוח

#### דוגמה:

```python
from spacy.lang.en.stop_words import STOP_WORDS
print("to" in STOP_WORDS)  # True
```

ניתן גם להוסיף או להסיר מילות עצירה בהתאמה אישית:

```python
nlp.vocab["awesome"].is_stop = True
nlp.vocab["the"].is_stop = False
```

### ה- Phrase Matching – זיהוי ביטויים בטקסט

ה- Phrase matching הוא תהליך של זיהוי ביטויים או רצפים קבועים של מילים בטקסט

ב־spaCy יש מחלקה מיוחדת לכך: `PhraseMatcher`

### דוגמה לשימוש:

```python
from spacy.matcher import PhraseMatcher
matcher = PhraseMatcher(nlp.vocab)

patterns = [nlp("Solar Power"), nlp("solarpower"), nlp("solar-power")]
matcher.add("SOLARPOWER", patterns)

doc = nlp("This building uses Solar-Power and solarpower systems")
matches = matcher(doc)
for match_id, start, end in matches:
    print(doc[start:end].text)
```

פלט:

```
Solar-Power
solarpower
```

### למה להשתמש בזה

* מאפשר לזהות שמות, ביטויים, מושגים
* שימושי בחיפוש, סיווג טקסטים, זיהוי ישויות

ה- PhraseMatcher יודע לעבוד בצורה חכמה עם מילים שונות, צורות דקדוקיות שונות וסימני פיסוק
