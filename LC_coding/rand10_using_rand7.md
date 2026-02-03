# Implement Rand10() Using Rand7()

## Problem Description
Given a function `rand7()` which generates a uniform random integer in the range $[1, 7]$, implement a function `rand10()` which generates a uniform random integer in the range $[1, 10]$.

You can only use `rand7()` to generate random numbers and you should minimize the number of calls to `rand7()`.

## Approach
To generate a larger range $[1, 10]$ from a smaller range $[1, 7]$, we can call `rand7()` twice to generate a range of $7 \times 7 = 49$ possible outcomes.

1.  **Generate a 2D-like coordinate**: 
    $val = (rand7() - 1) \times 7 + rand7()$
    This formula gives a uniform distribution in the range $[1, 49]$.
2.  **Rejection Sampling**:
    - We want a range that is a multiple of 10 to ensure uniformity. The largest multiple of 10 less than 49 is 40.
    - If $val \leq 40$, we can map it to $[1, 10]$ using `(val - 1) % 10 + 1`.
    - If $val > 40$, we discard the result and repeat the process.
3.  **Uniformity**: Since each number in $[1, 40]$ has a $1/49$ probability of occurring, each result in $[1, 10]$ will have a $4/49$ probability after one attempt. After considering the retries, the final probability for each number becomes exactly $1/10$.

## Code with Test Cases

```python
import random

def rand7():
    """Mock rand7 provided by the system"""
    return random.randint(1, 7)

def rand10():
    """Generates a random integer in [1, 10] using rand7"""
    while True:
        # Generate a number in [1, 49]
        row = rand7()
        col = rand7()
        idx = (row - 1) * 7 + col
        
        # If the number is in [1, 40], return the mapped value
        if idx <= 40:
            return (idx - 1) % 10 + 1
        
        # If idx > 40, we repeat (rejection sampling)

# --- Test Cases ---
if __name__ == "__main__":
    iterations = 100000
    counts = {i: 0 for i in range(1, 11)}
    
    for _ in range(iterations):
        res = rand10()
        counts[res] += 1
        
    print(f"Sampling results over {iterations} iterations:")
    expected_prob = 1/10
    for i in range(1, 11):
        observed_prob = counts[i] / iterations
        print(f"Value {i:2}: Observed={observed_prob:.4f}, Expected={expected_prob:.4f}")

    # Check for uniformity (simple threshold check)
    for i in range(1, 11):
        assert abs((counts[i] / iterations) - expected_prob) < 0.01
    print("Test Passed: Distribution is uniform!")
```

## Complexity Analysis
- **Time Complexity**:
    - **Average Case**: $O(1)$. The probability of success in one iteration is $40/49 \approx 0.816$. The expected number of calls to `rand7()` is $2 \times \frac{49}{40} = 2.45$.
    - **Worst Case**: $O(\infty)$ (theoretically, if we always hit $41-49$).
- **Space Complexity**: $O(1)$.

