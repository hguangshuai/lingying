# Weighted Random Sampling

## Problem Description (English)
Given a list of positive weights `weights`, where `weights[i]` represents the weight of the $i$-th element, implement a function or a class that performs **Weighted Random Sampling**. The probability of picking index $i$ should be:
$$P(i) = \frac{weights[i]}{\sum_{j=0}^{n-1} weights[j]}$$

## Approach
To efficiently sample based on weights, we use the **Prefix Sum + Binary Search** method:

1.  **Preprocessing**:
    - Compute a prefix sum array `prefix_sums` where `prefix_sums[i]` is the sum of all weights from index `0` to `i`.
    - The total sum of weights is the last element of the `prefix_sums` array.
2.  **Sampling**:
    - Generate a random number `r` in the range `[0, total_sum)`.
    - Use **Binary Search** to find the first index $i$ such that `prefix_sums[i] > r`. This index corresponds to the sampled weight.

## Code with Test Cases

```python
import random
import bisect

class WeightedSampler:
    def __init__(self, weights):
        """
        Initializes the sampler with a list of weights.
        :param weights: List[int] or List[float]
        """
        self.prefix_sums = []
        current_sum = 0
        for w in weights:
            current_sum += w
            self.prefix_sums.append(current_sum)
        self.total_sum = current_sum

    def sample(self):
        """
        Returns a sampled index based on the weights.
        :return: int (index)
        """
        if self.total_sum == 0:
            return None
        
        # Generate a random float between 0 and total_sum
        target = random.uniform(0, self.total_sum)
        
        # Use binary search to find the index
        # bisect_right finds the first index where prefix_sum > target
        index = bisect.bisect_right(self.prefix_sums, target)
        return index

# --- Test Cases ---
if __name__ == "__main__":
    weights = [1, 3, 2] # Probabilities: 1/6, 3/6 (0.5), 2/6 (~0.33)
    sampler = WeightedSampler(weights)
    
    # Run simulation to verify probabilities
    iterations = 100000
    counts = {0: 0, 1: 0, 2: 0}
    for _ in range(iterations):
        idx = sampler.sample()
        counts[idx] += 1
    
    print(f"Weights: {weights}")
    print(f"Results after {iterations} iterations:")
    for i in range(len(weights)):
        observed_prob = counts[i] / iterations
        expected_prob = weights[i] / sum(weights)
        print(f"Index {i}: Observed={observed_prob:.4f}, Expected={expected_prob:.4f}")
```

## Complexity Analysis
- **Time Complexity**:
    - **Initialization**: $O(n)$ to build the prefix sum array.
    - **Sampling**: $O(\log n)$ per sample using binary search.
- **Space Complexity**: $O(n)$ to store the prefix sum array.

