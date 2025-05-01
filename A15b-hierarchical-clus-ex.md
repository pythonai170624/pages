
# Hierarchical Clustering דוגמא בפייתון

```python
import pandas as pd
import seaborn as sns
import matplotlib.pyplot as plt
from sklearn.preprocessing import MinMaxScaler
from sklearn.cluster import AgglomerativeClustering
from scipy.cluster.hierarchy import linkage, dendrogram

# 1. טוענים את הדאטה
df = pd.read_csv("iris_unlabaled.csv")

# 2. מנקים רק עמודות מספריות
df_numeric = df.select_dtypes(include=['float64', 'int64'])

# 3. סקיילינג (נרמול) עם MinMaxScaler
scaler = MinMaxScaler()
df_scaled = pd.DataFrame(scaler.fit_transform(df_numeric), columns=df_numeric.columns)

# 4. clustermap עם דנדרוגרמות
sns.clustermap(df_scaled, cmap="viridis", figsize=(10, 8))
plt.title("Clustermap with Dendrograms")
plt.show()

# 5. מטריצת קורלציה ו-heatmap
corr_matrix = df_scaled.corr()
sns.heatmap(corr_matrix, annot=True, cmap="coolwarm")
plt.title("Correlation Matrix")
plt.show()

# 6. מוצאים את שתי התכונות עם הקורלציה הכי גבוהה
top_corr = corr_matrix.where(~(corr_matrix == 1)).unstack().sort_values(ascending=False).dropna().reset_index()
top_corr.columns = ['Feature1', 'Feature2', 'Correlation']
top_pair = top_corr.iloc[0]

# 7. בונים לינקג' עם שיטת ward
linkage_matrix = linkage(df_scaled, method='ward')

# 8. מציירים דנדרוגרמה
plt.figure(figsize=(12, 6))
dendrogram(linkage_matrix)
plt.title("Dendrogram (Ward Linkage)")
plt.xlabel("Data Point Index")
plt.ylabel("Distance")
plt.show()

# 9. מבצעים clustering היררכי עם 3 קבוצות
model = AgglomerativeClustering(n_clusters=3)
df['Cluster'] = model.fit_predict(df_scaled)

# 10. מציירים פיזור של הקלאסטרים לפי שתי התכונות הכי מתואמות
plt.figure(figsize=(8, 6))
sns.scatterplot(data=df, x=top_pair['Feature1'], y=top_pair['Feature2'], hue='Cluster', palette='Set2')
plt.title("Clusters Based on Most Correlated Features")
plt.xlabel(top_pair['Feature1'])
plt.ylabel(top_pair['Feature2'])
plt.grid(True)
plt.show()

```
