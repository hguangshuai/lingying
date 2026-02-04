# Logistic Regression from Scratch

## Problem Description
Implement **Logistic Regression** from scratch using only `numpy`. The implementation should include the sigmoid function, the gradient descent update rule, and a prediction method.

## Approach
1.  **Hypothesis**: Use the Sigmoid function $\sigma(z) = \frac{1}{1 + e^{-z}}$ to map linear predictions to probabilities.
2.  **Loss Function**: Use **Log-Loss** (Binary Cross Entropy):
    $$J(w) = -\frac{1}{n} \sum [y \log(\hat{y}) + (1-y) \log(1-\hat{y})]$$
3.  **Gradient Descent**: Update weights $w$ and bias $b$ using the gradients:
    - $dw = \frac{1}{n} \mathbf{X}^T (\hat{y} - y)$
    - $db = \frac{1}{n} \sum (\hat{y} - y)$
4.  **Prediction**: Return 1 if $\sigma(\mathbf{w}^T\mathbf{x} + b) \ge 0.5$, else 0.

## Code with Test Cases

```python
import numpy as np

class LogisticRegressionScratch:
    def __init__(self, lr=0.01, iters=1000):
        self.lr = lr
        self.iters = iters
        self.weights = None
        self.bias = None

    def _sigmoid(self, z):
        return 1 / (1 + np.exp(-z))

    def fit(self, X, y):
        n_samples, n_features = X.shape
        # Initialize weights and bias
        self.weights = np.zeros(n_features)
        self.bias = 0

        # Gradient Descent
        for _ in range(self.iters):
            # 1. Forward pass: compute linear combination and sigmoid
            linear_model = np.dot(X, self.weights) + self.bias
            y_predicted = self._sigmoid(linear_model)

            # 2. Compute gradients
            dw = (1 / n_samples) * np.dot(X.T, (y_predicted - y))
            db = (1 / n_samples) * np.sum(y_predicted - y)

            # 3. Update parameters
            self.weights -= self.lr * dw
            self.bias -= self.lr * db

    def predict_prob(self, X):
        linear_model = np.dot(X, self.weights) + self.bias
        return self._sigmoid(linear_model)

    def predict(self, X):
        probs = self.predict_prob(X)
        return [1 if i >= 0.5 else 0 for i in probs]

# --- Test Case ---
if __name__ == "__main__":
    # Small synthetic dataset
    X = np.array([[1, 2], [2, 3], [3, 4], [6, 5], [7, 8], [8, 9]])
    y = np.array([0, 0, 0, 1, 1, 1])

    model = LogisticRegressionScratch(lr=0.1, iters=1000)
    model.fit(X, y)
    
    # Test on training data
    predictions = model.predict(X)
    print(f"Weights: {model.weights}")
    print(f"Bias: {model.bias:.4f}")
    print(f"Actual:      {y.tolist()}")
    print(f"Predictions: {predictions}")
```

## Complexity Analysis
- **Time Complexity**: $O(I \cdot n \cdot d)$
    - $I$: Number of iterations.
    - $n$: Number of samples.
    - $d$: Number of features.
- **Space Complexity**: $O(d)$ to store the weights.

