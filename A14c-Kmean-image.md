# כיווץ צבעים בתמונה בעזרת קיי-מינס

## מבוא

כמות הצבעים בתמונה דיגיטלית יכולה להיות עצומה – לפעמים מאות אלפים של גוונים שונים  
אבל לפעמים אנחנו רוצים לצמצם את כמות הצבעים למספר קטן יותר – למשל 4, 10 או 25 צבעים עיקריים בלבד  
המטרה: לשמר את הצורה והפרטים המרכזיים של התמונה, אבל עם הרבה פחות צבעים

לשם כך נשתמש באלגוריתם של **למידה לא מונחית** מסוג **קיי-מינס**, שיחלק את הפיקסלים בקובץ התמונה לקבוצות דומות לפי הצבע  
כל קבוצה תיוצג על ידי **צבע ממוצע אחד**, וכל הפיקסלים בקבוצה יקבלו את הצבע הזה


<img src="dog_image.jpeg" style="width: 50%" />

---

## קוד פייתון לביצוע קיי-מינס על תמונה

```python
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.image as mpimg
from sklearn.cluster import KMeans
from PIL import Image

# Load the image
image = Image.open("dog_image.jpeg")
image_np = np.array(image)

# Save original shape
original_shape = image_np.shape

# Reshape to (num_pixels, 3)
pixels = image_np.reshape(-1, 3)

# Define K values to test
k_values = [4, 10, 25, 50]

# Loop over each K and generate quantized image
for k in k_values:
    # Fit KMeans on pixel RGB values
    kmeans = KMeans(n_clusters=k, random_state=42)
    kmeans.fit(pixels)

    # Replace each pixel with the centroid color of its cluster
    new_colors = kmeans.cluster_centers_[kmeans.labels_]
    new_image = new_colors.reshape(original_shape).astype(np.uint8)

    # Plot result
    plt.figure(figsize=(6, 4))
    plt.imshow(new_image)
    plt.title(f"Image with K = {k} Colors")
    plt.axis("off")
    plt.tight_layout()
    plt.show()
