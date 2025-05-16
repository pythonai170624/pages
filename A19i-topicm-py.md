---

## הקוד המלא

```python
# Load the data
import pandas as pd
import numpy as np

df = pd.read_csv('npr.csv')
df.head()

# Look at a specific document
df['Article'][0]

# Feature extraction with count vectorization
from sklearn.feature_extraction.text import CountVectorizer

X = df['Article']
count_vectorizer = CountVectorizer(max_df=0.9, min_df=2, stop_words='english')
X_counts = count_vectorizer.fit_transform(X)

# Train the LDA model
from sklearn.decomposition import LatentDirichletAllocation

lda_model = LatentDirichletAllocation(n_components=7, random_state=101)
lda_model.fit(X_counts)

# Examine the count vectorizer tokens
print(len(count_vectorizer.get_feature_names_out()))
print(type(count_vectorizer.get_feature_names_out()))

# Check the LDA model components
lda_model.components_.shape

# Look at the components (probabilities of tokens for each topic)
lda_model.components_

# Example of using np.argsort
array = np.array([10, 2, 5])
np.argsort(array)

# Get the top 10 tokens for the first topic
first_topic = lda_model.components_[0]
np.argsort(first_topic)[-10:]

# Get the top 10 tokens for each topic
for topic_idx, topic in enumerate(lda_model.components_):
    topic_token_array = []
    for index in topic.argsort()[-10:]:
        topic_token_array.append(count_vectorizer.get_feature_names_out()[index])
    print(f"Topic number {topic_idx} - Top 10 tokens:")
    print(topic_token_array)
    print()

# Get the topic distribution for the documents
topic_results = lda_model.transform(X_counts)
topic_results[0]  # Probability distribution for the first document

# Assign the most likely topic to each document
df['topic'] = topic_results.argmax(axis=1)
df.head()
```