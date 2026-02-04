# Linear Regression vs. Logistic Regression

## English Description / Topic
Comparing **Linear Regression** and **Logistic Regression**, with a specific focus on why Logistic Regression lacks a closed-form solution and requires iterative methods like Gradient Descent.

## Fundamental Knowledge

### 1. Linear Regression
- **Purpose**: Predicts a continuous output (e.g., house prices).
- **Model**: $y = \mathbf{w}^T\mathbf{x} + b$.
- **Loss Function**: Mean Squared Error (MSE), which is a quadratic function.
- **Closed-form Solution**: Yes. The **Normal Equation** $\mathbf{w} = (\mathbf{X}^T\mathbf{X})^{-1}\mathbf{X}^T\mathbf{y}$ provides the exact solution by setting the gradient of MSE to zero (it's a linear system of equations).

### 2. Logistic Regression
- **Purpose**: Predicts the probability of a categorical outcome (e.g., Spam vs. Not Spam).
- **Model**: $y = \sigma(\mathbf{w}^T\mathbf{x} + b)$, where $\sigma(z) = \frac{1}{1 + e^{-z}}$ is the sigmoid function.
- **Loss Function**: Log-Loss / Binary Cross-Entropy (BCE).
- **Closed-form Solution**: **No**. Setting the gradient of the Log-Loss function to zero results in a non-linear transcendental equation that cannot be solved algebraically for $\mathbf{w}$.

## Spoken / Interview Answer

### "What is the main difference between the two?"
"Linear Regression is used for regression tasks to predict continuous values, whereas Logistic Regression is a classification algorithm used to predict probabilities of classes. Linear regression fits a straight line to the data, while Logistic Regression fits an S-shaped sigmoid curve to squash the output between 0 and 1."

### "Why do we use the Logistic (Sigmoid) Function?"
"We use the sigmoid function for several key reasons:
1. **Probability Mapping**: It squashes any real-valued input into the range $(0, 1)$, which is perfect for representing probabilities.
2. **Differentiability**: It is smooth and differentiable everywhere, which is essential for gradient-based optimization.
3. **Nice Derivative**: The derivative $\sigma'(z) = \sigma(z)(1 - \sigma(z))$ makes the gradient calculations very efficient.
4. **Binary Classification**: It naturally models the Bernoulli distribution, which is the foundation for binary classification."

### "Can Logistic Regression have a closed-form solution?"
"No, Logistic Regression does not have a closed-form solution like the Normal Equation in Linear Regression. 

The reason is the **Sigmoid function**. When we take the derivative of the Log-Loss function and set it to zero to find the minimum, the resulting equation is non-linear because $\mathbf{w}$ is inside the sigmoid. Specifically, the gradient is $\sum ( \sigma(\mathbf{w}^T\mathbf{x}_i) - y_i )\mathbf{x}_i = 0$. This is a transcendental equation, and there is no algebraic way to isolate $\mathbf{w}$.

Therefore, we must use iterative optimization methods such as **Stochastic Gradient Descent (SGD)** or **Newton's Method (Iteratively Reweighted Least Squares - IRLS)** to find the optimal weights."

### "Why is the lack of a closed-form solution not necessarily a problem?"
"In practice, iterative methods are often preferred even for Linear Regression on very large datasets because calculating the matrix inverse $(\mathbf{X}^T\mathbf{X})^{-1}$ in the closed-form solution is computationally expensive ($O(d^3)$) and can be numerically unstable if the matrix is singular (collinearity)."

---

## Logistic Regression Deep Dive

### 1. Key Assumptions
- **Linearity of Independent Variables and Log-Odds**: The relationship between the input features and the log-odds (logit) must be linear.
- **Independence of Errors**: Observations should be independent of each other.
- **No Multicollinearity**: Independent variables should not be highly correlated with each other.

### 2. Decision Boundary
- The decision boundary for Logistic Regression is **Linear**. 
- Specifically, it is the set of points where $P(y=1|x) = 0.5$, which corresponds to $\mathbf{w}^T\mathbf{x} + b = 0$. This defines a hyperplane in the feature space.

### 3. Multi-class Extension (Softmax Regression)
- To handle more than two classes, we use **Multinomial Logistic Regression** (also known as Softmax Regression).
- Instead of a single sigmoid, we use the **Softmax function**, which outputs a probability distribution over $K$ classes:
  $$P(y=k|x) = \frac{e^{\mathbf{w}_k^T\mathbf{x}}}{\sum_{j=1}^K e^{\mathbf{w}_j^T\mathbf{x}}}$$

### 4. Why "Logistic" if it's for Classification?
- It is called "regression" because it performs regression on the **probability** of the class. The underlying model predicts the log-odds, which is a continuous value, but we apply a threshold (e.g., 0.5) to use it for classification.

