# GMM and EM Algorithm

## English Description / Topic
Why **Gaussian Mixture Models (GMM)** cannot be solved directly with MLE and a detailed breakdown of the **Expectation-Maximization (EM)** algorithm.

## Fundamental Knowledge

### 1. What is GMM?
A Gaussian Mixture Model represents a dataset as a weighted sum of multiple Gaussian distributions.
$$P(x) = \sum_{k=1}^K \pi_k \mathcal{N}(x | \mu_k, \sigma_k^2)$$
Where $\pi_k$ is the mixing weight for component $k$.

### 2. Why can't we use direct MLE?
In standard MLE, we take the log of the likelihood and differentiate. For GMM, the likelihood is:
$$L = \prod_{i=1}^n \left( \sum_{k=1}^K \pi_k \mathcal{N}(x_i | \mu_k, \sigma_k^2) \right)$$
When we take the **log**, the summation remains **inside** the log:
$$\ln L = \sum_{i=1}^n \ln \left( \sum_{k=1}^K \pi_k \mathcal{N}(x_i | \mu_k, \sigma_k^2) \right)$$
**The Problem**: You cannot distribute the $\ln$ over the $\sum$. Taking the derivative of this expression results in complex, coupled equations where parameters of all clusters depend on each other, making it impossible to solve for $\mu$ or $\sigma$ analytically (no closed-form solution).

### 3. The EM Algorithm Solution
EM introduces **Latent Variables** $z_{ik}$ (the probability that point $i$ belongs to cluster $k$).

#### Step 1: Initialization
Pick initial values for $\mu_k, \sigma_k^2,$ and $\pi_k$.

#### Step 2: E-Step (Expectation)
Calculate the "responsibility" (posterior probability) that cluster $k$ generated data point $x_i$:
$$\gamma(z_{ik}) = \frac{\pi_k \mathcal{N}(x_i | \mu_k, \sigma_k^2)}{\sum_{j=1}^K \pi_j \mathcal{N}(x_i | \mu_j, \sigma_j^2)}$$

#### Step 3: M-Step (Maximization)
Update the parameters to maximize the expected likelihood using the weights from the E-step:
- **Mean**: $\mu_k^{new} = \frac{\sum \gamma(z_{ik}) x_i}{\sum \gamma(z_{ik})}$ (Weighted average)
- **Variance**: $\sigma_k^{2, new} = \frac{\sum \gamma(z_{ik}) (x_i - \mu_k)^2}{\sum \gamma(z_{ik})}$
- **Weights**: $\pi_k^{new} = \frac{\sum \gamma(z_{ik})}{n}$

#### Step 4: Convergence
Repeat E and M steps until the log-likelihood stabilizes.

## Spoken / Interview Answer
"We can't use simple MLE for GMM because of the **'Sum-under-the-Log'** problem. In a single Gaussian, the log cancels the exponent, making math easy. In GMM, the log applies to a *sum* of Gaussians, which prevents us from isolating the parameters and leads to a non-convex optimization problem with no closed-form solution.

To solve this, we use the **EM Algorithm**. Think of it as an iterative version of MLE. In the **E-step**, we estimate the 'membership' of each data point—basically asking, 'Which Gaussian do you likely belong to?'. In the **M-step**, we treat those memberships as fixed and update the $\mu$ and $\sigma$ of each Gaussian using a weighted MLE. By alternating these steps, we are guaranteed to increase the likelihood until we reach a local optimum."


