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

### פרמטרים

```python
doc1 = nlp("I am a runner running in a race because I love to run since I ran today")

print(f'{"token.text":16}', f'{"token.pos_":15}', f'{"token.lemma":23}', f'{"token.lemma_":20}')
print('-' * 70)

for token in doc1:
    print(f'{token.text:10}', '\t', f'{token.pos_:10}', '\t', f'{token.lemma:10}', '\t', token.lemma_)

```

Ouput:
```
token.text       token.pos_      token.lemma             token.lemma_        
----------------------------------------------------------------------
I          	 PRON       	 4690420944186131903 	 I
am         	 AUX        	 10382539506755952630 	 be
a          	 DET        	 11901859001352538922 	 a
runner     	 NOUN       	 12640964157389618806 	 runner
running    	 VERB       	 12767647472892411841 	 run
in         	 ADP        	 3002984154512732771 	 in
a          	 DET        	 11901859001352538922 	 a
race       	 NOUN       	 8048469955494714898 	 race
because    	 SCONJ      	 16950148841647037698 	 because
I          	 PRON       	 4690420944186131903 	 I
love       	 VERB       	 3702023516439754181 	 love
to         	 PART       	 3791531372978436496 	 to
run        	 VERB       	 12767647472892411841 	 run
since      	 SCONJ      	 10066841407251338481 	 since
I          	 PRON       	 4690420944186131903 	 I
ran        	 VERB       	 12767647472892411841 	 run
today      	 NOUN       	 11042482332948150395 	 today
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
