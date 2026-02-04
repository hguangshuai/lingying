# Why Linear and Logistic Regression are Related (GLM Perspective)

## English Description / Topic
Explaining why **Linear Regression** and **Logistic Regression** are fundamentally the same mathematical framework, specifically within the context of **Generalized Linear Models (GLMs)**.

## Fundamental Knowledge

### 1. The Common Core: Linear Combination
At their hearts, both models use a linear combination of input features and weights:
$$z = \mathbf{w}^T\mathbf{x} + b$$
The difference lies in how this linear signal $z$ is mapped to the final output $y$.

### 2. The GLM Framework (Generalized Linear Models)
A GLM consists of three components:
1.  **Systemic Component**: The linear predictor $\eta = \mathbf{w}^T\mathbf{x}$.
2.  **Link Function $g(\mu)$**: Connects the linear predictor to the expected value of the response: $g(\mu) = \eta$.
3.  **Random Component**: The probability distribution of the target variable (Exponential Family).

### 3. Comparison through GLM
| Feature | Linear Regression | Logistic Regression |
| :--- | :--- | :--- |
| **Random Component** | **Gaussian** (Normal) | **Bernoulli** (Binomial) |
| **Link Function** | **Identity**: $\mu = \eta$ | **Logit**: $\ln(\frac{\mu}{1-\mu}) = \eta$ |
| **Inverse Link** | $\hat{y} = z$ | $\hat{y} = \sigma(z) = \frac{1}{1+e^{-z}}$ |
| **Loss Function** | MSE (from Gaussian MLE) | BCE (from Bernoulli MLE) |

---

## Mathematical Connection

### 1. The "Linear" in Logistic Regression
Logistic Regression is actually a linear model for the **Log-Odds** (Logit):
$$\ln\left( \frac{P(y=1|x)}{P(y=0|x)} \right) = \mathbf{w}^T\mathbf{x} + b$$
If you treat the "Log-Odds" as your target, the model is identical to Linear Regression.

### 2. Maximum Likelihood Origin
Both models' loss functions are derived from **MLE**:
- Minimizing **MSE** is equivalent to maximizing the likelihood of data assuming **Gaussian noise**.
- Minimizing **Binary Cross-Entropy** is equivalent to maximizing the likelihood of data assuming **Bernoulli noise**.

## Spoken / Interview Answer
"Mathematically, Linear and Logistic regression are both members of the **Generalized Linear Models (GLM)** family. They share the same **Systemic Component**, which is the linear combination $\mathbf{w}^T\mathbf{x}$. 

The reason they look different is the **Link Function**. 
In **Linear Regression**, we assume the noise is Gaussian, and use an **Identity** link function, so the output is directly the linear sum. 
In **Logistic Regression**, we assume the noise is Bernoulli (binary), and we use the **Logit** link function. This means Logistic Regression is actually a **linear model for the log-odds**. 

So, they aren't 'different' models; they are the same linear tool applied to different probability distributions of the target variable."

