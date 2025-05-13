# 🌳 Decision Tree Exercise (CART)

## Problem Description

You are given a dataset from an online store. For each customer, we know:

* Whether they received a discount
* Whether they made a purchase
* Whether they returned to the store

The goal is to build a **decision tree** using the **CART algorithm** that predicts whether a customer will return to the store

## 🔢 Dataset

| Discount | Purchase | Returned |
| -------- | -------- | -------- |
| Yes      | Yes      | Yes      |
| Yes      | No       | No       |
| No       | Yes      | Yes      |
| No       | No       | No       |
| Yes      | Yes      | Yes      |
| No       | No       | No       |


## ❓ Tasks

4. Use the trained model to **make a prediction** for a new customer where Discount=Yes, Purchase=No

5. Build a decision tree based on the dataset.

6. Use the **CART algorithm** with **Gini Index**.

7. Write Python code to build and visualize the decision tree using scikit-learn.

---

## 💪 Steps

1. Convert categorical values using `get_dummies()`
2. Train a `DecisionTreeClassifier` with the data
3. Visualize the tree using `plot_tree`

---

## 🐍 Python Solution (Using scikit-learn and get\_dummies)

```python
import pandas as pd
from sklearn.tree import DecisionTreeClassifier, plot_tree
import matplotlib.pyplot as plt

# Dataset
data = pd.DataFrame({
    'Discount': ['Yes', 'Yes', 'No', 'No', 'Yes', 'No'],
    'Purchase': ['Yes', 'No', 'Yes', 'No', 'Yes', 'No'],
    'Returned': ['Yes', 'No', 'Yes', 'No', 'Yes', 'No']
})

```

## 🧩 Challenge:

* Add a new customer who did not purchase but returned
* Try using `criterion='entropy'` instead of `gini` and compare the tree
