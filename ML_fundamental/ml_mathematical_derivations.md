# Comprehensive ML Mathematical Derivations

This document provides detailed derivations for the fundamental mathematical frameworks in Machine Learning, including MLE, MAP, and the mechanics of Boosting/GBDT.

---

## 1. MLE vs. MAP Estimation

### Maximum Likelihood Estimation (MLE)
**Goal**: Find the parameter $\theta$ that maximizes the probability of the observed data $X$.
$$ \hat{\theta}_{MLE} = \arg\max_{\theta} P(X|\theta) $$
- **Intuition**: We treat $\theta$ as a fixed but unknown constant and find the value that makes the observed data most likely.
- **Process**: 
    1. Define Likelihood $L(\theta) = \prod P(x_i|\theta)$.
    2. Take Log-Likelihood $\ell(\theta) = \sum \ln P(x_i|\theta)$.
    3. Solve $\frac{\partial \ell}{\partial \theta} = 0$.

### Maximum A Posteriori (MAP)
**Goal**: Find the parameter $\theta$ that is most probable given the data $X$, incorporating a **Prior** $P(\theta)$.
Using Bayes' Theorem: $P(\theta|X) = \frac{P(X|\theta)P(\theta)}{P(X)}$
Since $P(X)$ is constant with respect to $\theta$:
$$ \hat{\theta}_{MAP} = \arg\max_{\theta} P(X|\theta)P(\theta) = \arg\max_{\theta} [\ln P(X|\theta) + \ln P(\theta)] $$
- **Relation to Regularization**:
    - If the Prior $P(\theta)$ is **Gaussian** ($\mathcal{N}(0, \sigma^2)$), MAP is equivalent to **L2 Regularization (Ridge)**.
    - If the Prior $P(\theta)$ is **Laplacian**, MAP is equivalent to **L1 Regularization (Lasso)**.

---

## 2. Gradient Boosting Decision Trees (GBDT)

### The Core Idea
GBDT is an additive model that fits a new tree to the **negative gradient** (residuals) of the loss function.
$$ F_m(x) = F_{m-1}(x) + \gamma_m h_m(x) $$

### Derivation Steps
1.  **Objective**: Minimize $\sum_{i=1}^n L(y_i, F_{m-1}(x_i) + h_m(x_i))$.
2.  **Taylor Expansion (1st Order)**: 
    $L(y, F_{m-1} + h) \approx L(y, F_{m-1}) + \frac{\partial L(y, F_{m-1})}{\partial F_{m-1}} h$
3.  To minimize this, we want $h$ to move in the direction of the **negative gradient**:
    $$ r_{im} = -\left[ \frac{\partial L(y_i, F(x_i))}{\partial F(x_i)} \right]_{F=F_{m-1}} $$
4.  **Fitting**: Train a regression tree $h_m(x)$ to predict these "pseudo-residuals" $r_{im}$.
5.  **Special Case (MSE)**: If $L = \frac{1}{2}(y-F)^2$, then $-\frac{\partial L}{\partial F} = (y-F)$. The gradient is exactly the residual.

---

## 3. XGBoost: 2nd Order Derivation

XGBoost improves GBDT by using a **2nd Order Taylor Expansion** of the loss function.

### Objective Function
$$ \text{Obj}^{(m)} = \sum_{i=1}^n [L(y_i, F_{m-1}(x_i)) + g_i h_m(x_i) + \frac{1}{2} h_i h_m^2(x_i)] + \Omega(h_m) $$
Where:
- $g_i = \frac{\partial L(y_i, F_{m-1})}{\partial F_{m-1}}$ (Gradient)
- $h_i = \frac{\partial^2 L(y_i, F_{m-1})}{\partial F_{m-1}^2}$ (Hessian)

### Optimal Leaf Weight
For a fixed tree structure, the optimal weight $w_j$ for leaf $j$ is:
$$ w_j^* = -\frac{\sum_{i \in I_j} g_i}{\sum_{i \in I_j} h_i + \lambda} $$
And the resulting loss (Gain) is:
$$ \text{Gain} = \frac{1}{2} \left[ \frac{(\sum g_L)^2}{\sum h_L + \lambda} + \frac{(\sum g_R)^2}{\sum h_R + \lambda} - \frac{(\sum g_L + \sum g_R)^2}{\sum h_L + \sum h_R + \lambda} \right] - \gamma $$

---

## 4. AdaBoost: Weight Update Derivation

AdaBoost focuses on incorrectly classified samples by increasing their weights.

### Alpha ($\alpha$) Calculation
The importance of tree $m$ is calculated based on its error $\epsilon_m$:
$$ \alpha_m = \frac{1}{2} \ln \left( \frac{1 - \epsilon_m}{\epsilon_m} \right) $$

### Weight Update Rule
For each sample $i$:
$$ D_{m+1}(i) = \frac{D_m(i) \exp(-\alpha_m y_i h_m(x_i))}{Z_m} $$
- If correctly classified ($y_i h_m = 1$), weight decreases.
- If incorrectly classified ($y_i h_m = -1$), weight increases exponentially.

