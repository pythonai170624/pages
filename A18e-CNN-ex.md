## 🐶🐱 אימון רשת CNN לסיווג כלב או חתול (Colab)

## 🚀 Getting Started in Google Colab

To run this notebook in the cloud, open:
[https://colab.research.google.com/](https://colab.research.google.com/)

Create a new notebook by clicking **File → New notebook** and copy the code blocks below step-by-step

## CNN_dog_cat_dataset.zip

🗂️ Before running the training code, **upload the file** `CNN_dog_cat_dataset.zip` to your **Google Drive** inside a folder named `content`  
(Or modify the code to reflect a different folder structure if needed)

This dataset is required for training the CNN to classify dog and cat images

## CNN code

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
Output:
```python
Num GPUs Available:  1
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

Output:
```
single_prediction/  test_set/  training_set/
```

מציג את תוכן התיקייה הנוכחית ומוודא שהקבצים והמבנה תקינים

### 🖼️ הגדרת קונפיגורציה לטעינת תמונות

```python
from tensorflow.keras.preprocessing.image import ImageDataGenerator

# Training data generator with augmentation
train_datagen = ImageDataGenerator(
    rescale=1./255,         # Normalize pixel values to [0,1] — helps speed up training and stabilize gradients
    shear_range=0.2,        # Apply a slight diagonal transformation (shear) — simulates natural changes in camera angle
    zoom_range=0.2,         # Apply random zoom-in effect — helps the model recognize objects at different scales
    horizontal_flip=True    # Flip images horizontally — helps the model handle symmetry (e.g., cat facing left or right)
)

# Testing data generator — only normalization, no augmentation
test_datagen = ImageDataGenerator(rescale=1./255)
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

**🧠 מה זה Conv2D ו־MaxPooling2D ולמה משתמשים בהם?**

בבניית רשת עצבית קונבולוציונית (CNN), יש שתי שכבות חשובות במיוחד:

#### 🔷 Conv2D – שכבת קונבולוציה

קונבולוציה2ד היא שכבה שמחפשת **מאפיינים חשובים** בתמונה כמו קווים, צבעים, גבולות, תבניות, מרקם ועוד  
היא עושה את זה בעזרת **פילטרים קטנים** (למשל בגודל 3x3) שזזים על פני התמונה ובודקים כל אזור בנפרד

בכל פעם שהפילטר עובר על אזור בתמונה, הוא מחשב **עד כמה המאפיין הזה קיים שם**  
התוצאה היא **מפת מאפיינים** שמדגישה איפה נמצאים הדברים החשובים

#### 🔷 MaxPooling2D – שכבת מיצוע

 מקספול2ג שכבה שאחרי הקונבולוציה, ומטרתה **להקטין את גודל הנתונים**  
במקום לשמור את כל הערכים ממפת המאפיינים, היא לוקחת **את הערך הכי גבוה מכל אזור קטן** (למשל חלון 2 על 2)

למה זה טוב?
- חיסכון בחישובים
- הפחתת סיכון לאובר־פיטינג
- שמירה רק על המאפיינים החזקים ביותר

#### 🤔 אז למה משתמשים בזה כאן?

המטרה שלנו היא לזהות האם תמונה היא של **כלב או חתול**  
- Conv2D עוזרת לזהות תבניות חשובות כמו עיניים, פרווה, אוזניים...
- MaxPooling2D שומרת רק את מה שחשוב ומפשטת את התמונה

ביחד, הן עוזרות למודל ללמוד **ייצוג חכם של התמונה**, לפני שהוא מקבל החלטה סופית


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
