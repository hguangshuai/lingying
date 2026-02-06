# K-Means Clustering

## Problem Description
Implement the **K-Means Clustering** algorithm. Given a set of points and an integer $K$, partition the points into $K$ clusters such that each point belongs to the cluster with the nearest mean (centroid).

## Approach
K-Means is an iterative algorithm that follows these steps:

1.  **Initialization**: Randomly pick $K$ points from the dataset as initial centroids.
2.  **Assignment Step**: For each point, calculate its Euclidean distance to all centroids and assign it to the cluster with the nearest centroid.
3.  **Update Step**: For each cluster, recompute the centroid by taking the mean of all points assigned to that cluster.
4.  **Convergence**: Repeat the Assignment and Update steps until the centroids no longer change significantly (or a maximum number of iterations is reached).

## Code with Test Cases

```python
import numpy as np
import random

class KMeans:
    def __init__(self, k=3, max_iters=100, tolerance=1e-4):
        self.k = k
        self.max_iters = max_iters
        self.tolerance = tolerance
        self.centroids = None

    def fit(self, X):
        # 1. Initialization: Randomly pick k data points as initial centroids
        n_samples, n_features = X.shape
        random_indices = random.sample(range(n_samples), self.k)
        self.centroids = X[random_indices]

        for i in range(self.max_iters):
            # 2. Assignment Step: Find nearest centroid for each point
            clusters = [[] for _ in range(self.k)]
            labels = np.zeros(n_samples)
            
            for idx, x in enumerate(X):
                distances = np.linalg.norm(x - self.centroids, axis=1)
                cluster_idx = np.argmin(distances)
                labels[idx] = cluster_idx
                clusters[cluster_idx].append(x)

            # 3. Update Step: Recompute centroids
            prev_centroids = self.centroids.copy()
            for j in range(self.k):
                if clusters[j]:
                    self.centroids[j] = np.mean(clusters[j], axis=0)
            
            # 4. Convergence check
            diff = np.linalg.norm(self.centroids - prev_centroids)
            if diff < self.tolerance:
                print(f"Converged at iteration {i}")
                break
        
        return labels

# --- Test Cases ---
if __name__ == "__main__":
    # Create synthetic data: 3 clusters in 2D
    X1 = np.random.randn(50, 2) + np.array([5, 5])
    X2 = np.random.randn(50, 2) + np.array([-5, -5])
    X3 = np.random.randn(50, 2) + np.array([5, -5])
    X = np.vstack((X1, X2, X3))

    model = KMeans(k=3)
    labels = model.fit(X)

    print("Final Centroids:")
    print(model.centroids)
    
    # Verify that we have roughly 50 points in each cluster
    unique, counts = np.unique(labels, return_counts=True)
    print("Points per cluster:", dict(zip(unique, counts)))
```

## Complexity Analysis
- **Time Complexity**: $O(I \cdot K \cdot n \cdot d)$
    - $I$: Number of iterations.
    - $K$: Number of clusters.
    - $n$: Number of data points.
    - $d$: Number of dimensions (features).
- **Space Complexity**: $O(n \cdot d + K \cdot d)$ to store the data and the centroids.


