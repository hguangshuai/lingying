# Overfitting, Underfitting, and Regularization

## English Description / Topic
Definitions of **Overfitting** and **Underfitting**, strategies to mitigate overfitting, and a detailed comparison between **L1 (Lasso)** and **L2 (Ridge)** regularization.

## Fundamental Knowledge

### 1. Overfitting vs. Underfitting
- **Underfitting (High Bias)**: The model is too simple to capture the underlying patterns in the data. It performs poorly on both training and test sets.
- **Overfitting (High Variance)**: The model is too complex and "memorizes" the noise in the training data. It performs well on training data but poorly on test data.

### 2. How to Detect Overfitting
- **Learning Curves**: Plot training loss and validation loss against the number of epochs. Overfitting is detected when the **training loss continues to decrease but the validation loss starts to increase**.
- **Performance Gap**: A significant difference between training accuracy (e.g., 99%) and validation/test accuracy (e.g., 85%).
- **Cross-Validation**: If the model performs very differently on different folds, it might be overfitting to specific patterns in the data.

### 2. How to Solve Overfitting
- **Regularization**: Add a penalty term to the loss function to discourage large weights.
- **More Data**: Collecting more diverse data helps the model generalize better.
- **Feature Selection**: Remove irrelevant or redundant features.
- **Cross-Validation**: Use techniques like K-fold to ensure the model generalizes.
- **Simpler Model**: Reduce the number of layers, neurons, or polynomial degrees.
- **Early Stopping**: Stop training once the validation error starts to increase.
- **Dropout (for NN)**: Randomly deactivate neurons during training.

### 3. L1 vs. L2 Regularization (Formulas)
- **L2 Regularization (Ridge)**: 
    - Loss: $J(w) = Loss_{data} + \frac{\lambda}{2} \sum_{i=1}^n w_i^2$
    - Forces weights to be small but rarely exactly zero.
    - Good for handling collinearity.
- **L1 Regularization (Lasso)**: 
    - Loss: $J(w) = Loss_{data} + \lambda \sum_{i=1}^n |w_i|$
    - Encourages **sparsity** (sets many weights to exactly zero).
    - Acts as a built-in **feature selector**.

### 4. Why L1 for Feature Selection and L2 not?
- **Geometric Interpretation**: In 2D, the L2 constraint is a **circle** ($w_1^2 + w_2^2 \le C$), while the L1 constraint is a **diamond** ($|w_1| + |w_2| \le C$). 
- The loss function's contours are more likely to touch the "corners" of the L1 diamond on the axes (where one weight is 0) than the smooth L2 circle.
- **Mathematical Intuition**: The derivative of $|w|$ is constant ($\pm 1$), pushing the weight towards zero with a constant force even when the weight is very small. The derivative of $w^2$ is $2w$, which decreases as $w$ approaches zero, so the "push" vanishes.

### 5. Why not L3, L4, etc.?
- **Norm $L_p$ where $p > 2$**: As $p$ increases, the penalty for large weights becomes much more aggressive, but it provides no sparsity (the shape becomes a "squircle" and eventually a square as $p \to \infty$).
- **Complexity**: Higher-order norms are more computationally expensive to differentiate.
- **Non-Convexity ($p < 1$)**: Norms where $0 < p < 1$ (like $L_{0.5}$) provide even more sparsity than L1, but they make the optimization problem **non-convex**, meaning we are not guaranteed to find a global minimum. L1 is the smallest $p$ that remains convex.

## Spoken / Interview Answer

### "What are Overfitting and Underfitting?"
"Overfitting is when a model fits the training data too closely, including the noise, leading to poor generalization on new data. It's a 'high variance' problem. Underfitting is when the model is too simple to learn the data's structure, leading to poor performance even on the training set. It's a 'high bias' problem."

### "How do you solve Overfitting?"
"The most common ways are adding **Regularization** (L1/L2), getting **more training data**, or using **Cross-validation** to monitor performance. For deep learning, we also use **Dropout** or **Early Stopping**."

### "Difference between L1 and L2 Regularization?"
"The main difference is that **L1 (Lasso)** encourages sparsity, meaning it can drive some weight coefficients to exactly zero. This makes it very useful for **feature selection**. **L2 (Ridge)**, on the other hand, penalizes the square of the weights, forcing them to be small but not necessarily zero. L2 is generally better if you have many features that all contribute slightly to the output."

### "Why does L1 cause sparsity while L2 doesn't?"
"This is because of the shape of the constraint. The L1 penalty has 'corners' at the axes (a diamond shape in 2D), where the loss function's contours are more likely to hit a vertex where one coordinate is zero. The L2 penalty is a circle (hypersphere), so the solution typically lands somewhere on the curve where all weights are non-zero."

