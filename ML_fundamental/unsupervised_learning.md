# Unsupervised Learning

## English Description / Topic
Definition of **Unsupervised Learning**, common tasks, and popular algorithms.

## Fundamental Knowledge

### 1. Definition
Unsupervised Learning is a type of machine learning where the algorithm is trained on data that has **no pre-existing labels**. The goal is to discover hidden patterns, structures, or relationships within the raw data.

### 2. Common Tasks & Algorithms
- **Clustering**: Grouping similar data points together.
    - **K-Means**: Centroid-based clustering.
    - **Hierarchical Clustering**: Building a tree of clusters.
    - **DBSCAN**: Density-based clustering (good for non-spherical shapes).
- **Dimensionality Reduction**: Reducing the number of random variables under consideration.
    - **PCA (Principal Component Analysis)**: Linear reduction that maximizes variance.
    - **t-SNE / UMAP**: Non-linear reduction used for visualization.
- **Association Rule Learning**: Discovering rules that describe your data (e.g., "People who buy milk also buy bread").
    - **Apriori Algorithm**.
- **Anomaly Detection**: Identifying unusual data points (can be unsupervised).
    - **Isolation Forest**.

### 3. Key Differences from Supervised Learning
- **Input**: Only $X$, no $y$ (target).
- **Goal**: Exploration and structure discovery vs. Prediction and mapping.
- **Evaluation**: Much harder because there is no "ground truth" to compare against. Metrics like the **Silhouette Score** (for clustering) are used instead of Accuracy.

## Spoken / Interview Answer
"Unsupervised Learning is about finding the 'hidden structure' in data without the help of labels. The most common example is **Clustering**, where we group similar users or products together. 
Another critical use case is **Dimensionality Reduction** using PCA, which helps in simplifying data, removing noise, and improving the performance of other downstream models. 
The biggest challenge with unsupervised learning is **evaluation**—since we don't have ground truth labels, we have to rely on internal metrics like the elbow method or silhouette score to decide if our clusters are meaningful."

