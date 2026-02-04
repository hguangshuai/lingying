# KNN from Scratch

## Problem Description
Implement the **K-Nearest Neighbors (KNN)** algorithm for classification. Given a training dataset and a test point, find the $K$ closest training points and return the majority label.

## Approach
KNN is a **lazy learning** algorithm, meaning it doesn't have a dedicated training phase. All computation happens during prediction:

1.  **Calculate Distances**: Compute the Euclidean distance between the test point and all points in the training set.
2.  **Sort**: Sort the distances in ascending order.
3.  **Pick K**: Select the top $K$ nearest neighbors.
4.  **Majority Vote**: Count the labels of these $K$ neighbors and return the most common one.

## Code with Test Cases

```python
import numpy as np
from collections import Counter

class KNN:
    def __init__(self, k=3):
        self.k = k

    def fit(self, X, y):
        # KNN just stores the training data
        self.X_train = X
        self.y_train = y

    def predict(self, X_test):
        # Predict labels for a list of test points
        return [self._predict_single(x) for x in X_test]

    def _predict_single(self, x):
        # 1. Calculate Euclidean distances to all training points
        distances = [np.linalg.norm(x - x_train) for x_train in self.X_train]
        
        # 2. Get indices of the k nearest neighbors
        k_indices = np.argsort(distances)[:self.k]
        
        # 3. Get the labels of those k neighbors
        k_nearest_labels = [self.y_train[i] for i in k_indices]
        
        # 4. Majority vote
        most_common = Counter(k_nearest_labels).most_common(1)
        return most_common[0][0]

# --- Test Case ---
if __name__ == "__main__":
    # Training data (2 features)
    X_train = np.array([[1, 2], [1.5, 1.8], [5, 8], [8, 8], [1, 0.6], [9, 11]])
    y_train = [0, 0, 1, 1, 0, 1] # Labels: 0 or 1

    # Test data
    X_test = np.array([[2, 2], [7, 7]])

    knn = KNN(k=3)
    knn.fit(X_train, y_train)
    predictions = knn.predict(X_test)

    print(f"Test points: {X_test.tolist()}")
    print(f"Predictions: {predictions}")
    # Expected: [0, 1]
```

## Complexity Analysis
- **Time Complexity**:
    - **Training**: $O(1)$. It only stores the data.
    - **Prediction**: $O(n \cdot d)$, where $n$ is the number of training samples and $d$ is the number of features. This is because we must calculate the distance to every training point.
- **Space Complexity**: $O(n \cdot d)$ to store the training dataset.

