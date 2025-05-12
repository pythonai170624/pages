### Removing Stop Words in spaCy

בדומה להוספה של מילת עצירה לרשימת ברירת המחדל, ניתן גם **להסיר מילת עצירה קיימת** אם נרצה שהיא תיחשב כחלק מהתוכן המשמעותי

#### דוגמה להסרת stop word:

```python
nlp.Defaults.stop_words.remove('beyond')
nlp.vocab['beyond'].is_stop = False

print(nlp.vocab['beyond'].is_stop)  # False
```

### הסבר:

* השורה הראשונה מסירה את המילה `'beyond'` מהסט של מילות העצירה
* לאחר מכן אנו מציינים במפורש שהמילה אינה עוד stop word על ידי הגדרת `is_stop = False`
* אפשר לבדוק את הסטטוס עם `nlp.vocab['beyond'].is_stop`

התוצאה: spaCy לא תתייחס יותר ל־`beyond` כמילת עצירה

🖼️ *כאן להוסיף את התמונה מהשקף על הסרת stop word*
