## 🐶🐱 אימון רשת CNN לסיווג כלב או חתול (Colab)

## 🚀 Getting Started in Google Colab

To run this notebook in the cloud, open:
[https://colab.research.google.com/](https://colab.research.google.com/)

Upload your `.ipynb` file or create a new notebook and copy the code blocks below step-by-step.

### 🔧 Importing Required Libraries and Configuring GPU (if available)

```python
# Importing standard libraries
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import tensorflow as tf

# Optional: prevent TensorFlow from using GPU (for debugging or testing)
tf.config.set_visible_devices([], 'GPU')

# Check how many GPUs are available
print("Num GPUs Available: ", len(tf.config.experimental.list_physical_devices('GPU')))
```

Explanation:

* `numpy`, `pandas` are used for data handling
* `matplotlib`, `seaborn` are used for plotting and visualization
* `tensorflow` is the deep learning framework used to build and train the CNN
* `tf.config.set_visible_devices([], 'GPU')` disables GPU usage intentionally (can be removed to use GPU)
* `list_physical_devices('GPU')` checks how many GPUs are detected by TensorFlow\` – מציג כמה כרטיסי GPU זמינים

### 📁 התחברות ל־Google Drive

כדי להשתמש בקבצים מהדרייב, מחברים את Colab אל חשבון Google Drive שלך

```python
import pandas as pd
import zipfile
from google.colab import drive

drive.mount('/content/drive')  # מחבר את הדרייב, יווצר קישור לנתיב /content/drive
```

### 📌 בדיקת מיקום נוכחי

...
