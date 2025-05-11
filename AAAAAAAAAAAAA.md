### Analyzing Token Parameters in spaCy

In this example, we're processing a sentence that includes multiple forms of the word `run`:

```python
doc1 = nlp("I am a runner running in a race because I love to run since I ran today")

for token in doc1:
    print(token.text, '\t', token.pos_, '\t', token.lemma, '\t', token.lemma_)
```

This line prints the following information for each token:

#### 1. `token.text`

The actual word (token) as it appears in the sentence

#### 2. `token.pos_`

The part-of-speech tag (POS) as a **readable string**, like:

* `PRON` – pronoun
* `AUX` – auxiliary verb
* `VERB` – main verb
* `NOUN` – noun

This is the **human-readable** label for grammatical role

#### 3. `token.lemma`

The lemma of the token, as an **internal hash-based ID number** (spaCy uses numerical IDs internally for efficiency)
This is useful for internal lookup but not for human interpretation

#### 4. `token.lemma_`

The lemma as a **readable string**, e.g.:

* `running`, `ran`, `run` → all will have `lemma_ = "run"`
* `am` → `lemma_ = "be"`

### Example Output (simplified):

```
I       PRON    4690420944186131903    I
am      AUX     10382539506755952630   be
runner  NOUN    5566430174578461922    runner
running VERB    976151799503001163    run
...
```

### Summary

This format helps analyze:

* What each word is (text)
* Its grammatical role (POS)
* The root/base form (lemma)

Using `lemma_` is most helpful when comparing different forms of a word like `run`, `running`, `ran`, `runs`, etc.
