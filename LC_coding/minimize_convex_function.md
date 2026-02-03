# Minimize Convex Function

## Approach 1: Ternary Search
Similar to Binary Search, it reduces the search space in each step, but instead of two parts, it divides the range into three.

1.  **Divide**: Pick two points $m_1$ and $m_2$ such that:
    - $m_1 = l + (r - l) / 3$
    - $m_2 = r - (r - l) / 3$
2.  **Compare**:
    - If $f(m_1) < f(m_2)$: The minimum must be to the left of $m_2$. New range: $[l, m_2]$.
    - If $f(m_1) > f(m_2)$: The minimum must be to the right of $m_1$. New range: $[m_1, r]$.
3.  **Terminate**: Repeat until $r - l < \epsilon$.

## Approach 2: Golden Section Search
This is an optimization of Ternary Search. By using the **Golden Ratio** $\phi = \frac{\sqrt{5}-1}{2} \approx 0.618$, we can ensure that in each iteration, we only need to evaluate the function at **one** new point instead of two. This is the most efficient derivative-free optimization method.

## Approach 3: Binary Search on Derivative (Gradient)
If the function $f(x)$ is **differentiable**, its derivative $f'(x)$ will be **monotonically increasing** for a convex function. 
1. The minimum occurs where $f'(x) = 0$.
2. We can use standard **Binary Search** to find the root of $f'(x)$.
3. This is much faster than Ternary Search because it halves the interval in each step using only one evaluation of the derivative.

## Code Implementations

```python
import math

# --- Solution 1: Ternary Search ---
def minimize_ternary(f, l, r, epsilon=1e-9):
    while r - l > epsilon:
        m1 = l + (r - l) / 3
        m2 = r - (r - l) / 3
        if f(m1) < f(m2):
            r = m2
        else:
            l = m1
    return (l + r) / 2

# --- Solution 2: Golden Section Search ---
def minimize_golden_section(f, l, r, epsilon=1e-9):
    phi = (math.sqrt(5) - 1) / 2  # ~0.618
    
    x1 = r - phi * (r - l)
    x2 = l + phi * (r - l)
    f1, f2 = f(x1), f(x2)

    while r - l > epsilon:
        if f1 < f2:
            r = x2
            x2 = x1
            f2 = f1
            x1 = r - phi * (r - l)
            f1 = f(x1)  # Only one new evaluation
        else:
            l = x1
            x1 = x2
            f1 = f2
            x2 = l + phi * (r - l)
            f2 = f(x2)  # Only one new evaluation
            
    return (l + r) / 2

# --- Solution 3: Binary Search on Derivative ---
def minimize_binary_gradient(df, l, r, epsilon=1e-9):
    """ df is the derivative function f'(x) """
    while r - l > epsilon:
        mid = (l + r) / 2
        if df(mid) < 0: # Sloping down, move right
            l = mid
        else: # Sloping up, move left
            r = mid
    return (l + r) / 2

# --- Test Cases ---
if __name__ == "__main__":
    f = lambda x: (x - 2)**2 + 5
    df = lambda x: 2 * (x - 2) # Derivative for f
    
    print(f"Ternary:        {minimize_ternary(f, -10, 10):.6f}")
    print(f"Golden Section: {minimize_golden_section(f, -10, 10):.6f}")
    print(f"Binary Grad:    {minimize_binary_gradient(df, -10, 10):.6f}")
```

## Complexity Analysis
- **Ternary Search**: $O(\log_{1.5}(\frac{range}{\epsilon}))$, 2 function calls per iteration.
- **Golden Section Search**: $O(\log_{1.618}(\frac{range}{\epsilon}))$, **1 function call** per iteration (more efficient).
- **Binary Search on Gradient**: $O(\log_{2}(\frac{range}{\epsilon}))$, 1 derivative call per iteration (fastest if derivative is easy to compute).
