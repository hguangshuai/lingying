# Maximum Likelihood Estimation (MLE) & Gradient Descent

## 1. Maximum Likelihood Estimation (MLE)

### Formula
Given a set of i.i.d. (independent and identically distributed) observations $X = \{x_1, x_2, \dots, x_n\}$ from a distribution with parameter $\theta$, the Likelihood function is:
$$L(\theta) = P(X|\theta) = \prod_{i=1}^n P(x_i|\theta)$$
The goal of MLE is to find $\hat{\theta}$ that maximizes this likelihood:
$$\hat{\theta}_{MLE} = \arg\max_{\theta} L(\theta)$$

### Why Log-Likelihood?
In practice, we maximize the **Log-Likelihood** $\ell(\theta) = \ln L(\theta)$ because:
1.  **Simplifies Calculation**: It turns products into sums: $\ln(\prod a_i) = \sum \ln a_i$.
2.  **Numerical Stability**: Products of many small probabilities can lead to arithmetic underflow; sums are more stable.
3.  **Monotonicity**: Since $\ln(x)$ is a monotonically increasing function, the $\theta$ that maximizes $L(\theta)$ also maximizes $\ln L(\theta)$.

### Derivation Example: Bernoulli Distribution
Suppose $x_i \in \{0, 1\}$ and $P(x=1) = p$.
1.  **Likelihood**: $L(p) = \prod p^{x_i} (1-p)^{1-x_i} = p^{\sum x_i} (1-p)^{n - \sum x_i}$
2.  **Log-Likelihood**: $\ell(p) = (\sum x_i) \ln p + (n - \sum x_i) \ln(1-p)$
3.  **Derivative**: Set $\frac{d\ell}{dp} = 0$:
    $$\frac{\sum x_i}{p} - \frac{n - \sum x_i}{1-p} = 0$$
4.  **Solve**:
    $$(1-p)\sum x_i = p(n - \sum x_i)$$
    $$\sum x_i - p\sum x_i = np - p\sum x_i \implies \hat{p} = \frac{\sum x_i}{n} = \bar{x}$$

---

## 2. Gradient Descent (GD)

### Formula
Gradient Descent is an iterative optimization algorithm to find the minimum of a cost function $J(w)$. The update rule is:
$$w_{t+1} = w_t - \eta \nabla J(w_t)$$
Where:
- $w$: Parameters (weights).
- $\eta$: Learning rate (step size).
- $\nabla J(w)$: Gradient (vector of partial derivatives).

### Derivation (via Taylor Expansion)
Why do we move in the **negative** gradient direction?
Suppose we are at point $w$ and we move a small step $\Delta w$. The new value of the function $J(w + \Delta w)$ can be approximated using the **1st-order Taylor Expansion**:
$$J(w + \Delta w) \approx J(w) + \nabla J(w)^T \Delta w$$
To minimize $J$, we want $J(w + \Delta w) < J(w)$, which means we need:
$$\nabla J(w)^T \Delta w < 0$$
The dot product $A \cdot B$ is minimized when $B$ is in the **opposite direction** of $A$. Specifically, if we set $\Delta w = -\eta \nabla J(w)$ (where $\eta > 0$):
$$\nabla J(w)^T (-\eta \nabla J(w)) = -\eta \|\nabla J(w)\|^2$$
Since $\|\nabla J(w)\|^2$ is always non-negative, this choice of $\Delta w$ ensures (for small enough $\eta$) that the function value decreases.

### Types of Gradient Descent
1.  **Batch Gradient Descent (BGD)**: Uses the entire dataset to compute the gradient. Accurate but slow for large data.
2.  **Stochastic Gradient Descent (SGD)**: Uses one random sample at a time. Fast but noisy; helps escape local minima.
3.  **Mini-batch Gradient Descent**: Uses a small subset (batch size). A compromise between BGD and SGD; most commonly used in Deep Learning.

---

## Spoken / Interview Answer
"**MLE** is a method to estimate model parameters by finding values that make the observed data most likely. We usually maximize the log-likelihood because it turns products into sums, making differentiation easier. For example, for a Bernoulli distribution, MLE shows the best parameter is just the sample mean.

**Gradient Descent** is the optimization engine used when we can't solve for parameters directly (like in Neural Networks). We update weights by moving in the opposite direction of the gradient. Mathematically, this is derived from the Taylor expansion, showing that the negative gradient is the direction of steepest descent. We use a learning rate $\eta$ to control the step size to ensure we don't overstep the minimum."

