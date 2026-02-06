# Poisson Distribution: Bus Arrival Probability

## Problem Description (English)
**Scenario**: At a bus station, you observe that 8 buses arrive in a 6-minute interval. What is the probability that exactly 1 bus arrives in a subsequent 3-minute interval?

## Approach
This is a classic problem that can be modeled using the **Poisson Distribution**. The Poisson distribution expresses the probability of a given number of events occurring in a fixed interval of time if these events occur with a known constant mean rate and independently of the time since the last event.

### 1. Identify the average rate ($\lambda$)
- The observed rate is 8 buses per 6 minutes.
- Therefore, the average rate for a 3-minute interval is:
  $$\lambda = \frac{8 \text{ buses}}{6 \text{ minutes}} \times 3 \text{ minutes} = 4 \text{ buses}$$

### 2. Poisson Formula
The probability of observing $k$ events in an interval is:
$$P(k; \lambda) = \frac{\lambda^k e^{-\lambda}}{k!}$$

In this case:
- $k = 1$ (we want to find the probability of exactly 1 bus)
- $\lambda = 4$

### 3. Calculation
$$P(1; 4) = \frac{4^1 \cdot e^{-4}}{1!} = 4 \cdot e^{-4}$$
Using $e \approx 2.718$, $e^{-4} \approx 0.0183$:
$$P(1; 4) \approx 4 \cdot 0.0183 = 0.0732 \text{ (or } 7.32\%)$$

## Code with Test Cases

```python
import math

def poisson_probability(k, lambd):
    """
    Calculates the Poisson probability P(k; lambd)
    """
    return (lambd**k * math.exp(-lambd)) / math.factorial(k)

# --- Test Case ---
if __name__ == "__main__":
    # Observed: 8 buses in 6 mins -> lambda = 4 for 3 mins
    lambd_3min = (8 / 6) * 3
    k = 1
    
    prob = poisson_probability(k, lambd_3min)
    
    print(f"Average rate (lambda) for 3 minutes: {lambd_3min}")
    print(f"Probability of exactly {k} bus in 3 minutes: {prob:.4f} ({prob*100:.2f}%)")

    # Verification: sum of probabilities from 0 to infinity should be 1
    # We check the sum of first 20 terms as an approximation
    total_prob = sum(poisson_probability(i, lambd_3min) for i in range(20))
    print(f"Sum of probabilities (0-19): {total_prob:.6f}")
```

## Complexity Analysis
- **Time Complexity**: $O(1)$ for a single probability calculation (assuming `exp` and `factorial` are $O(1)$ or small constant).
- **Space Complexity**: $O(1)$.


