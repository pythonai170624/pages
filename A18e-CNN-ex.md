## 🐶🐱 אימון רשת CNN לסיווג כלב או חתול (Colab)

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

```bash
pwd
```

פקודה שמציגה את הנתיב הנוכחי של סביבת העבודה
לרוב התוצאה תהיה:

```
/content
```

### 📦 חילוץ קובץ ZIP עם התמונות

```python
import zipfile

zip_file = '/content/drive/MyDrive/content/CNN_dog_cat_dataset.zip'  # הנתיב לקובץ הדחוס בדרייב

with zipfile.ZipFile(zip_file, 'r') as zip_ref:
  zip_ref.extractall('/content/dataset')  # חילוץ הקבצים לתיקייה חדשה
```

### 📂 מעבר לתיקייה עם הדאטה

```bash
cd dataset/
```

```bash
cd CNN_dog_cat_dataset/
```

בתוך התיקייה הזו יופיעו ככל הנראה תיקיות בשם `train` ו־`test` המכילות את התמונות של כלבים וחתולים

### 🧪 בדיקת התיקייה לאחר חילוץ

```bash
ls
```

מציג את תוכן התיקייה הנוכחית ומוודא שהקבצים והמבנה תקינים

### 🖼️ הגדרת קונפיגורציה לטעינת תמונות

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

train_datagen = ImageDataGenerator(
    rescale=1./255,         # נירמול ערכי הפיקסלים לטווח [0,1]
    shear_range=0.2,        # סיבוב קל בצורת התמונה (shear)
    zoom_range=0.2,         # ביצוע זום אקראי פנימה
    horizontal_flip=True    # היפוך אופקי של התמונה
)

test_datagen = ImageDataGenerator(rescale=1./255)  # לדאטה של הבדיקה אין שינויים – רק נירמול
```

### 📥 טעינת התמונות מהתיקיות

```python
training_set = train_datagen.flow_from_directory(
    '/content/dataset/CNN_dog_cat_dataset/training_set',
    target_size=(64, 64),
    batch_size=32,
    class_mode='binary'
)

test_set = test_datagen.flow_from_directory(
    '/content/dataset/CNN_dog_cat_dataset/test_set',
    target_size=(64, 64),
    batch_size=32,
    class_mode='binary'
)
```

* `target_size=(64, 64)` → שינוי גודל התמונות לגודל אחיד
* `batch_size=32` → כמה תמונות נטענות בכל פעם
* `class_mode='binary'` → סיווג בינארי (חתול או כלב)

### 🧠 בניית רשת CNN

```python
from keras.models import Sequential
from keras.layers import Conv2D, MaxPooling2D, Flatten, Dense

cnn = Sequential()

# שכבת קונבולוציה ראשונה
cnn.add(Conv2D(filters=32, kernel_size=3, activation='relu', input_shape=[64, 64, 3]))
cnn.add(MaxPooling2D(pool_size=2, strides=2))

# שכבת קונבולוציה שנייה
cnn.add(Conv2D(filters=32, kernel_size=3, activation='relu'))
cnn.add(MaxPooling2D(pool_size=2, strides=2))

# Flatten
cnn.add(Flatten())

# Fully Connected
cnn.add(Dense(units=128, activation='relu'))
cnn.add(Dense(units=1, activation='sigmoid'))  # בגלל שזה סיווג בינארי
```

### ⚙️ קומפילציה ואימון המודל

```python
cnn.compile(optimizer='adam', loss='binary_crossentropy', metrics=['accuracy'])

cnn.fit(x=training_set, validation_data=test_set, epochs=25)
```

* `adam` → אלגוריתם אימון מתקדם
* `binary_crossentropy` → פונקציית עלות עבור סיווג בינארי
* `epochs=25` → הרשת תלמד במשך 25 מחזורים על כל הדאטה

### 🐾 חיזוי על תמונה חדשה

```python
import numpy as np
from keras.preprocessing import image

# טוענים את התמונה
test_image = image.load_img('/content/dataset/single_prediction/cat_or_dog.jpg', target_size=(64, 64))

# Convert image to array
test_image = image.img_to_array(test_image)

# Normalize pixel values (rescale to [0,1])
test_image = test_image / 255.0

# Expand dimensions to match the CNN model input shape
test_image = np.expand_dims(test_image, axis=0)

# Predict using the trained CNN model
result = cnn.predict(test_image)
print(result)

# Interpret the result
if result[0][0] > 0.5:
    prediction = 'dog'
else:
    prediction = 'cat'

print(prediction)
```

הסבר:

* `image.load_img` טוען את התמונה ומכווץ אותה לגודל המתאים
* `img_to_array` ממיר את התמונה למערך מספרי
* חילוק ב־255 מנרמל את ערכי הפיקסלים לטווח \[0,1]
* `expand_dims` מוסיף מימד כדי להתאים את הצורה שמצפה לה המודל (batch size)
* `cnn.predict` מחזיר הסתברות שהאובייקט הוא כלב
* ההשוואה מול 0.5 קובעת את הקטגוריה הסופית

אם `result[0][0] > 0.5` → המודל ינבא **כלב**
אם פחות → **חתול**
