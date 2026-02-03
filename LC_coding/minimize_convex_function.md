# Minimize Convex Function (Ternary Search)

## Problem Description
Given a **convex function** $f(x)$ and a range $[l, r]$, find the value of $x$ in this range that minimizes $f(x)$.

A convex function (in this context, often meaning unimodal with a single minimum) has the property that if you take any two points $x_1$ and $x_2$, the function decreases towards the minimum and then increases.

## Approach
To find the minimum of a continuous convex function, we use **Ternary Search**. Similar to Binary Search, it reduces the search space in each step, but instead of two parts, it divides the range into three.

1.  **Divide**: Pick two points $m_1$ and $m_2$ such that:
    - $m_1 = l + (r - l) / 3$
    - $m_2 = r - (r - l) / 3$
2.  **Compare**:
    - If $f(m_1) < f(m_2)$: The minimum must be to the left of $m_2$. The new range becomes $[l, m_2]$.
    - If $f(m_1) > f(m_2)$: The minimum must be to the right of $m_1$. The new range becomes $[m_1, r]$.
    - If $f(m_1) == f(m_2)$: The minimum is between $m_1$ and $m_2$. The new range becomes $[m_1, m_2]$.
3.  **Terminate**: Repeat until $r - l < \epsilon$, where $\epsilon$ is the desired precision (e.g., $10^{-9}$).

## Code with Test Cases

```python
def minimize_convex(f, l, r, epsilon=1e-9):
    """
    Finds the x that minimizes f(x) in range [l, r] using Ternary Search.
    """
    while r - l > epsilon:
        m1 = l + (r - l) / 3
        m2 = r - (r - l) / 3
        
        if f(m1) < f(m2):
            r = m2
        else:
            l = m1
            
    return (l + r) / 2

# --- Test Cases ---
if __name__ == "__main__":
    import math

    # Case 1: Quadratic function f(x) = (x - 2)^2 + 5. Min at x=2.
    f1 = lambda x: (x - 2)**2 + 5
    res1 = minimize_convex(f1, -10, 10)
    print(f"Test 1: f(x)=(x-2)^2+5, Range=[-10, 10] | Result: {res1:.6f}, Expected: 2.000000")

    # Case 2: f(x) = x^4 - 4x. Min at x=1.
    f2 = lambda x: x**4 - 4*x
    res2 = minimize_convex(f2, -5, 5)
    print(f"Test 2: f(x)=x^4-4x, Range=[-5, 5] | Result: {res2:.6f}, Expected: 1.000000")

    # Case 3: Absolute value f(x) = |x + 3|. Min at x=-3.
    f3 = lambda x: abs(x + 3)
    res3 = minimize_convex(f3, -10, 0)
    print(f"Test 3: f(x)=|x+3|, Range=[-10, 0] | Result: {res3:.6f}, Expected: -3.000000")

    # Case 4: Narrow range
    f4 = lambda x: (x - 0.123456)**2
    res4 = minimize_convex(f4, 0, 1)
    print(f"Test 4: f(x)=(x-0.123456)^2 | Result: {res4:.6f}, Expected: 0.123456")
```

## Complexity Analysis
- **Time Complexity**: $O(\log_{1.5}(\frac{r-l}{\epsilon}))$. In each iteration, we eliminate $1/3$ of the search space, so the range shrinks by a factor of $2/3$. The number of iterations depends on the initial range and the required precision.
- **Space Complexity**: $O(1)$ for the iterative approach.

