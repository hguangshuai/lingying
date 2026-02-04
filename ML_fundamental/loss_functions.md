# Common Loss Functions & Cross-Entropy

## English Description / Topic
Overview of common loss functions used in Machine Learning and a deep dive into **Cross-Entropy Loss**.

## Fundamental Knowledge

### 1. Regression Loss Functions
- **Mean Squared Error (MSE / L2 Loss)**: $\frac{1}{n} \sum (y_i - \hat{y}_i)^2$. 
    - Sensitive to outliers because it squares the error.
- **Mean Absolute Error (MAE / L1 Loss)**: $\frac{1}{n} \sum |y_i - \hat{y}_i|$. 
    - More robust to outliers.
- **Huber Loss**: A combination of MSE and MAE. It is quadratic for small errors and linear for large errors.

### 2. Classification Loss Functions
- **Binary Cross-Entropy (Log Loss)**: Used for binary classification.
  $$L = -\frac{1}{n} \sum [y_i \log(\hat{y}_i) + (1 - y_i) \log(1 - \hat{y}_i)]$$
- **Categorical Cross-Entropy**: Used for multi-class classification (Softmax output).
  $$L = -\sum y_{i,c} \log(\hat{y}_{i,c})$$
- **Hinge Loss**: Used in SVMs. $L = \max(0, 1 - y \cdot \hat{y})$. It encourages the model to not just be correct, but correct by a certain margin.

### 3. Deep Dive: What is Cross-Entropy?
- **Information Theory Origin**: Cross-entropy measures the difference between two probability distributions (the true labels $P$ and the predicted probabilities $Q$).
- **Intuition**: It penalizes the model heavily when it is confident but wrong. If the true label is 1 but the model predicts 0.01, the $\log(0.01)$ term becomes a very large negative number, resulting in a high loss.

## Spoken / Interview Answer

### "What loss functions are you familiar with?"
"For regression, I typically use **MSE** or **MAE**. MSE is standard but sensitive to outliers, so I might switch to MAE or **Huber Loss** if the data is noisy. For classification, **Cross-Entropy** is the go-to loss function, especially for neural networks, as it works naturally with Sigmoid or Softmax outputs."

### "Why use Cross-Entropy instead of MSE for classification?"
"There are two main reasons. First, **MSE for classification leads to a non-convex optimization problem** when used with a sigmoid activation, making it hard to find the global minimum. Second, **Cross-Entropy avoids the vanishing gradient problem** better; if the model is very wrong, the gradient of the MSE loss becomes very small (the 'saturation' of sigmoid), whereas Cross-Entropy maintains a steeper gradient that allows the model to learn faster."

