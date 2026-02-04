# Calculating Mean and Variance for Billion-Scale Data

## English Description / Topic
How to calculate the **Mean** and **Variance** of a massive dataset (e.g., > 1 billion records) that cannot fit into memory, focusing on numerical stability and parallelization.

## Fundamental Knowledge

### 1. The Naive Approach (And why it fails)
The standard formula for variance is:
$$\sigma^2 = \frac{\sum x_i^2}{n} - \left( \frac{\sum x_i}{n} \right)^2$$
**Problems**:
- **Catastrophic Cancellation**: If $\sum x_i^2$ and $(\sum x_i)^2/n$ are very large and very close to each other, subtracting them can lead to a massive loss of precision (floating-point error).
- **Overflow**: $\sum x_i^2$ can easily exceed the maximum value of a 64-bit float when $n$ is in the billions.

### 2. Welford's Online Algorithm (Single-Machine / Streaming)
Welford's algorithm calculates the mean and variance in a single pass while maintaining numerical stability.
For each new data point $x_n$:
1.  $\text{count} = n$
2.  $\delta = x_n - \mu_{n-1}$
3.  $\mu_n = \mu_{n-1} + \frac{\delta}{n}$
4.  $M2_n = M2_{n-1} + \delta \cdot (x_n - \mu_n)$
5.  $\text{Variance}_n = \frac{M2_n}{n}$ (or $n-1$ for sample variance)

### 3. Parallelization (Distributed Computing / MapReduce)
To calculate this across multiple machines, we compute local statistics and then merge them.
To merge two sets $A$ and $B$:
- $n_{total} = n_A + n_B$
- $\delta = \mu_B - \mu_A$
- $\mu_{total} = \mu_A + \delta \cdot \frac{n_B}{n_{total}}$
- $M2_{total} = M2_A + M2_B + \delta^2 \cdot \frac{n_A n_B}{n_{total}}$

## Code Implementation (MVP & Refined)

```python
class WelfordStatistics:
    def __init__(self):
        self.n = 0
        self.mu = 0.0
        self.m2 = 0.0

    def update(self, x):
        """Update stats with a single new value"""
        self.n += 1
        delta = x - self.mu
        self.mu += delta / self.n
        delta2 = x - self.mu
        self.m2 += delta * delta2

    def finalize(self):
        """Retrieve final mean and variance"""
        if self.n < 2:
            return self.mu, 0.0
        return self.mu, self.m2 / self.n

def merge_stats(stats_a, stats_b):
    """Merges two WelfordStatistics objects (Parallel/MapReduce step)"""
    n_total = stats_a.n + stats_b.n
    if n_total == 0: return stats_a
    
    delta = stats_b.mu - stats_a.mu
    new_mu = stats_a.mu + delta * (stats_b.n / n_total)
    new_m2 = stats_a.m2 + stats_b.m2 + (delta ** 2) * (stats_a.n * stats_b.n / n_total)
    
    res = WelfordStatistics()
    res.n = n_total
    res.mu = new_mu
    res.m2 = new_m2
    return res

# --- Test Case ---
if __name__ == "__main__":
    data = [10, 20, 30, 40, 50]
    
    # Single-pass calculation
    ws = WelfordStatistics()
    for val in data:
        ws.update(val)
    mean, var = ws.finalize()
    print(f"Single Pass - Mean: {mean}, Var: {var}") # Mean: 30, Var: 200

    # Parallel calculation simulation
    ws1 = WelfordStatistics()
    for val in data[:2]: ws1.update(val) # [10, 20]
    
    ws2 = WelfordStatistics()
    for val in data[2:]: ws2.update(val) # [30, 40, 50]
    
    merged = merge_stats(ws1, ws2)
    m_mean, m_var = merged.finalize()
    print(f"Parallel    - Mean: {m_mean}, Var: {m_var}")
    assert mean == m_mean and var == m_var
```

## Spoken / Interview Answer
"For a dataset with over a billion points, I would avoid the naive variance formula because it's numerically unstable and prone to overflow. Instead, I would use **Welford's Algorithm**. 
This algorithm maintains a running mean and a sum of squares of differences from the mean ($M2$). It only requires a single pass over the data and is very stable because it uses the difference between the current point and the moving average. 
To scale this to a distributed system like **Spark or MapReduce**, I would have each worker compute the $n, \mu,$ and $M2$ for its partition of the data, and then use a **Merging Formula** to combine these statistics into a global result. This way, we never need to store all billion points in memory at once."

