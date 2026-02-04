# MLE for Normal Distribution (Mean and Variance)

## English Description / Topic
Step-by-step mathematical derivation of **Maximum Likelihood Estimation (MLE)** for the parameters $\mu$ (mean) and $\sigma^2$ (variance) of a **Normal (Gaussian) Distribution**.

## Fundamental Knowledge

### 1. The Normal Distribution PDF
The probability density function (PDF) for a single observation $x_i$ is:
$$f(x_i; \mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( -\frac{(x_i - \mu)^2}{2\sigma^2} \right)$$

### 2. The Likelihood Function $L(\mu, \sigma^2)$
For $n$ i.i.d. observations $x_1, \dots, x_n$:
$$L(\mu, \sigma^2) = \prod_{i=1}^n \frac{1}{\sqrt{2\pi\sigma^2}} \exp\left( -\frac{(x_i - \mu)^2}{2\sigma^2} \right)$$
$$L(\mu, \sigma^2) = (2\pi\sigma^2)^{-n/2} \exp\left( -\frac{1}{2\sigma^2} \sum_{i=1}^n (x_i - \mu)^2 \right)$$

### 3. The Log-Likelihood $\ell(\mu, \sigma^2)$
$$\ell(\mu, \sigma^2) = -\frac{n}{2} \ln(2\pi) - \frac{n}{2} \ln(\sigma^2) - \frac{1}{2\sigma^2} \sum_{i=1}^n (x_i - \mu)^2$$

---

## Derivation Steps

### Step 1: Solving for $\mu$
Take the partial derivative of $\ell$ with respect to $\mu$ and set to zero:
$$\frac{\partial \ell}{\partial \mu} = -\frac{1}{2\sigma^2} \cdot 2 \cdot \sum_{i=1}^n (x_i - \mu) \cdot (-1) = 0$$
$$\frac{1}{\sigma^2} \sum_{i=1}^n (x_i - \mu) = 0 \implies \sum x_i - n\mu = 0$$
$$\hat{\mu}_{MLE} = \frac{1}{n} \sum_{i=1}^n x_i$$
**Result**: The MLE for $\mu$ is the **sample mean**.

### Step 2: Solving for $\sigma^2$
Take the partial derivative with respect to $\sigma^2$ (treating $\sigma^2$ as a single variable $\theta$):
$$\frac{\partial \ell}{\partial \sigma^2} = -\frac{n}{2\sigma^2} + \frac{1}{2(\sigma^2)^2} \sum_{i=1}^n (x_i - \mu)^2 = 0$$
Multiply by $2(\sigma^2)^2$:
$$-n\sigma^2 + \sum_{i=1}^n (x_i - \mu)^2 = 0$$
$$\hat{\sigma}^2_{MLE} = \frac{1}{n} \sum_{i=1}^n (x_i - \mu)^2$$
**Result**: The MLE for $\sigma^2$ is the **sample variance** (biased version with $1/n$).

---

## Spoken / Interview Answer
"To estimate the parameters of a Normal distribution, we use **MLE**. 
First, I write the **Joint Likelihood** as the product of individual Gaussian PDFs. 
Then, I take the **Log-Likelihood** to simplify the exponent. 
To find $\mu$, I take the partial derivative and find that it equals the **average of the samples**. 
To find $\sigma^2$, I take the partial derivative with respect to the variance and find that it equals the **average of the squared deviations** from the mean. 
One interesting detail is that the MLE for variance is technically **biased** because it divides by $n$ rather than $n-1$, which is why we often use Bessel's correction in other contexts, but for MLE, $1/n$ is the correct mathematical result."

