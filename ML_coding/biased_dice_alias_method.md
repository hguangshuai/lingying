# Simulate Biased Dice

## Problem Description (English)
Given a list of probabilities (or weights) $P = [p_1, p_2, \dots, p_n]$ such that $\sum p_i = 1$, simulate a biased dice. Calling the function should return index $i$ with probability $p_i$. 

Specifically, address:
1.  How to simulate it using standard methods.
2.  How to achieve **$O(1)$** time complexity for sampling after preprocessing.

## Approach

### 1. Prefix Sum + Binary Search ($O(\log n)$ sampling)
The standard approach involves creating a prefix sum array and using binary search to find the index corresponding to a random number $r \in [0, 1)$.

### 2. Alias Method ($O(1)$ sampling)
To achieve $O(1)$ sampling, we use the **Alias Method**.
- **Preprocessing**: We "level" the probabilities so that each of the $n$ bins has an average probability of $1/n$. Some bins will have an "excess" (probability $> 1/n$) and some will have a "deficit" (probability $< 1/n$). We use the excess from one bin to fill the deficit of another. 
- Each bin $i$ will eventually contain:
    - A portion of its original probability $p_i$.
    - An "alias" index $j$ that fills the remaining space in the bin.
- **Sampling**:
    1. Randomly pick a bin $i \in [0, n-1]$.
    2. Within that bin, pick between the original index $i$ and its alias $j$ based on a precomputed threshold.

## Code with Test Cases

```python
import random

class BiasedDiceO1:
    def __init__(self, probabilities):
        n = len(probabilities)
        self.n = n
        self.alias = [0] * n
        self.prob_threshold = [0.0] * n

        # Scaled probabilities: mean should be 1.0
        scaled_probs = [p * n for p in probabilities]
        
        small = []
        large = []
        for i, p in enumerate(scaled_probs):
            if p < 1.0:
                small.append(i)
            else:
                large.append(i)

        while small and large:
            s = small.pop()
            l = large.pop()
            
            self.prob_threshold[s] = scaled_probs[s]
            self.alias[s] = l
            
            # Reduce the large probability by the amount used to fill the small one
            scaled_probs[l] = (scaled_probs[l] + scaled_probs[s]) - 1.0
            
            if scaled_probs[l] < 1.0:
                small.append(l)
            else:
                large.append(l)

        # Fill remaining bins (should have 1.0 probability)
        while large:
            self.prob_threshold[large.pop()] = 1.0
        while small:
            self.prob_threshold[small.pop()] = 1.0

    def sample(self):
        i = random.randrange(self.n)
        if random.random() < self.prob_threshold[i]:
            return i
        else:
            return self.alias[i]

# --- Test Cases ---
if __name__ == "__main__":
    probs = [0.1, 0.2, 0.4, 0.3] # Expected: 10%, 20%, 40%, 30%
    dice = BiasedDiceO1(probs)
    
    iterations = 100000
    counts = {i: 0 for i in range(len(probs))}
    for _ in range(iterations):
        counts[dice.sample()] += 1
        
    print(f"Probabilities: {probs}")
    print(f"Results after {iterations} iterations (Alias Method):")
    for i in range(len(probs)):
        observed = counts[i] / iterations
        print(f"Index {i}: Observed={observed:.4f}, Expected={probs[i]:.4f}")
```

## Complexity Analysis
- **Preprocessing**: $O(n)$. We iterate through the probabilities and use two stacks (small/large) to build the alias table.
- **Sampling**: **$O(1)$**. It requires one integer random generation (to pick the bin) and one float random generation (to pick between the index and its alias).
- **Space Complexity**: $O(n)$ to store the `alias` and `prob_threshold` tables.

