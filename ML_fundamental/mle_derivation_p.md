# Maximum Likelihood Estimation (MLE) for Parameter p

## English Description / Topic
Step-by-step derivation of **Maximum Likelihood Estimation (MLE)** to estimate a parameter $p$ given a probability density/mass function. We will use the **Bernoulli Distribution** as the primary example.

## Fundamental Knowledge

### 1. The Scenario
Suppose we have a set of $n$ independent and identically distributed (i.i.d.) observations $x_1, x_2, \dots, x_n$ from a Bernoulli distribution:
- $P(X=1) = p$
- $P(X=0) = 1 - p$
- Probability Mass Function (PMF): $f(x; p) = p^x (1-p)^{1-x}$ for $x \in \{0, 1\}$.

### 2. The Likelihood Function $L(p)$
The likelihood is the joint probability of all observed data points, viewed as a function of the parameter $p$:
$$L(p) = \prod_{i=1}^n f(x_i; p) = \prod_{i=1}^n p^{x_i} (1-p)^{1-x_i}$$
$$L(p) = p^{\sum x_i} (1-p)^{n - \sum x_i}$$

### 3. The Log-Likelihood $\ell(p)$
To simplify the derivation, we take the natural logarithm:
$$\ell(p) = \ln(L(p)) = (\sum x_i) \ln(p) + (n - \sum x_i) \ln(1-p)$$

### 4. Maximizing the Log-Likelihood
Take the derivative of $\ell(p)$ with respect to $p$ and set it to zero:
$$\frac{d\ell}{dp} = \frac{\sum x_i}{p} - \frac{n - \sum x_i}{1 - p} = 0$$

### 5. Solving for $\hat{p}$
Multiply by $p(1-p)$ to clear the denominators:
$$(1-p)\sum x_i - p(n - \sum x_i) = 0$$
$$\sum x_i - p \sum x_i - pn + p \sum x_i = 0$$
$$\sum x_i - pn = 0$$
$$\hat{p}_{MLE} = \frac{\sum_{i=1}^n x_i}{n}$$

**Result**: The MLE for $p$ is simply the sample mean (the proportion of successes in the sample).

## Spoken / Interview Answer
"To estimate a parameter using MLE, we follow a four-step process. 
First, we write down the **Likelihood function**, which is the joint probability of our observed data. 
Second, we take the **natural log** of that function to turn products into sums, making it much easier to differentiate. 
Third, we take the **derivative** of the log-likelihood with respect to our parameter $p$ and set it to zero to find the critical point. 
Finally, we solve for $p$. In the case of a Bernoulli distribution, the math shows that the most likely value for $p$ is just the **sample mean**—for example, if you flip a biased coin 100 times and get 70 heads, the MLE estimate for the probability of heads is $0.7$."

