# Logistic Regression Derivation

## English Description / Topic
Step-by-step mathematical derivation of **Logistic Regression**, including the Sigmoid function, the Likelihood function, and the Gradient Descent update rule.

## Fundamental Knowledge

### 1. The Model (Sigmoid)
The probability of a binary outcome is modeled as:
$$P(y=1|x) = \sigma(\mathbf{w}^T\mathbf{x}) = \frac{1}{1 + e^{-\mathbf{w}^T\mathbf{x}}}$$
And $P(y=0|x) = 1 - \sigma(\mathbf{w}^T\mathbf{x})$.

### 2. Maximum Likelihood Estimation (MLE)
For $n$ independent observations, the Likelihood function $L(\mathbf{w})$ is:
$$L(\mathbf{w}) = \prod_{i=1}^n P(y_i|x_i) = \prod_{i=1}^n [\sigma(\mathbf{w}^T\mathbf{x}_i)]^{y_i} [1 - \sigma(\mathbf{w}^T\mathbf{x}_i)]^{1-y_i}$$

### 3. Log-Likelihood
To simplify, we take the natural log (Log-Likelihood):
$$\ell(\mathbf{w}) = \sum_{i=1}^n [y_i \ln(\sigma(\mathbf{w}^T\mathbf{x}_i)) + (1 - y_i) \ln(1 - \sigma(\mathbf{w}^T\mathbf{x}_i))]$$
The **Log-Loss** (Binary Cross Entropy) is simply the negative log-likelihood: $J(\mathbf{w}) = -\frac{1}{n}\ell(\mathbf{w})$.

### 4. Gradient Derivation
First, note the derivative of the sigmoid: $\frac{d\sigma(z)}{dz} = \sigma(z)(1 - \sigma(z))$.
The gradient of the log-likelihood for a single sample $i$ with respect to $w_j$:
$$\frac{\partial \ell}{\partial w_j} = \left( \frac{y_i}{\sigma(z_i)} - \frac{1-y_i}{1-\sigma(z_i)} \right) \frac{\partial \sigma(z_i)}{\partial w_j}$$
Substituting the sigmoid derivative:
$$\frac{\partial \ell}{\partial w_j} = \left( \frac{y_i}{\sigma(z_i)} - \frac{1-y_i}{1-\sigma(z_i)} \right) \sigma(z_i)(1-\sigma(z_i))x_{ij}$$
Simplifying:
$$\frac{\partial \ell}{\partial w_j} = [y_i(1-\sigma(z_i)) - (1-y_i)\sigma(z_i)]x_{ij} = (y_i - \sigma(\mathbf{w}^T\mathbf{x}_i))x_{ij}$$

### 5. Update Rule (Gradient Descent)
Since we want to *minimize* the loss $J(\mathbf{w}) = -\ell(\mathbf{w})$, we move in the direction of the negative gradient:
$$\mathbf{w}_{new} = \mathbf{w}_{old} + \eta \sum_{i=1}^n (y_i - \sigma(\mathbf{w}^T\mathbf{x}_i))\mathbf{x}_i$$
(Where $\eta$ is the learning rate).

## Spoken / Interview Answer
"To derive Logistic Regression, we start with the **Sigmoid function** to map inputs to probabilities. We then use **Maximum Likelihood Estimation (MLE)** to find the parameters that make the observed data most probable. By taking the log of the likelihood, we get the **Log-Likelihood** function. To find the optimal weights, we take the derivative of this function with respect to the weights. 
A key trick is the sigmoid derivative: $\sigma' = \sigma(1-\sigma)$. After simplification, the gradient ends up being very clean: it's the **error** (actual $y$ minus predicted $\hat{y}$) multiplied by the input $x$. Finally, we use **Gradient Descent** to iteratively update the weights."

