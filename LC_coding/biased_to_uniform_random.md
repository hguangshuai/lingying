# Biased to Uniform Random Sampling

## Problem Description
Given a method `getRandom01Biased()` that generates a random integer in $\{0, 1\}$, where $0$ is generated with probability $p$ and $1$ is generated with probability $1-p$ (where $0 < p < 1$).

Implement a method `getRandom06Uniform()` that generates a random integer in the range $[0, 6]$ (i.e., $\{0, 1, 2, 3, 4, 5, 6\}$) with **uniform probability** ($1/7$ each).

## Approach
To solve this, we use the **Von Neumann Trick** to first get a fair coin flip from a biased one, and then use bit manipulation to generate a larger range.

### Step 1: Get a Fair Coin (0 or 1 with 50/50 probability)
If we call `getRandom01Biased()` twice:
- $(0, 1)$ occurs with probability $p(1-p)$
- $(1, 0)$ occurs with probability $(1-p)p$
Since $p(1-p) = (1-p)p$, these two outcomes are equally likely. We can map $(0, 1)$ to `0` and $(1, 0)$ to `1`. If we get $(0, 0)$ or $(1, 1)$, we discard and try again.

### Step 2: Generate Range [0, 6]
To generate a number up to $6$, we need at least 3 bits ($2^3 = 8$ possible values).
1. Generate 3 bits using our fair coin to get a number in $[0, 7]$.
2. If the resulting number is $7$, discard it (rejection sampling) to keep only $[0, 6]$.
3. Each number in $\{0, \dots, 6\}$ will have a probability of exactly $1/7$.

## Code with Test Cases

```python
import random

# Mocking the biased generator
P = 0.3 # Example: 0 is generated with 0.3 probability
def getRandom01Biased():
    return 0 if random.random() < P else 1

def getFairBit():
    """Generates 0 or 1 with 50/50 probability using Von Neumann Trick"""
    while True:
        b1 = getRandom01Biased()
        b2 = getRandom01Biased()
        if b1 == 0 and b2 == 1:
            return 0
        if b1 == 1 and b2 == 0:
            return 1
        # Discard (0,0) and (1,1)

def getRandom06Uniform():
    """Generates [0, 6] uniformly using Rejection Sampling"""
    while True:
        # Generate 3 bits for a range [0, 7]
        num = (getFairBit() << 2) | (getFairBit() << 1) | getFairBit()
        if num <= 6:
            return num

# --- Test Cases ---
if __name__ == "__main__":
    iterations = 70000
    counts = {i: 0 for i in range(7)}
    
    for _ in range(iterations):
        res = getRandom06Uniform()
        counts[res] += 1
        
    print(f"Sampling results over {iterations} iterations (Biased p={P}):")
    expected_prob = 1/7
    for i in range(7):
        observed_prob = counts[i] / iterations
        print(f"Value {i}: Observed={observed_prob:.4f}, Expected={expected_prob:.4f}")
```

## Complexity Analysis
- **Time Complexity**:
    - **Expected**: $O(1)$. While it uses rejection sampling, the probability of success in each iteration is high ($2p(1-p)$ for the fair bit, and $7/8$ for the final number), so it converges very quickly.
    - **Worst Case**: $O(\infty)$ (theoretically, though practically impossible).
- **Space Complexity**: $O(1)$ excluding the recursion/call stack.

