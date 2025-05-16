# 🌳 Radnom forest exercise 

**The goal here is to compare random forst to decision tree**

Steps:

1. split train-test 70%-30%
2. build DecisionTreeClassifier and RandomForestClassifier (100 estimators)
3. use accuracy_score for both models
4. print the accuracy
5. create bar plot for comparison

Sample code:

```python
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.metrics import accuracy_score
import matplotlib.pyplot as plt

# large data set
X, y = make_classification(n_samples=10000, n_features=20, n_informative=15,
                           n_redundant=5, n_classes=2, random_state=42)
```