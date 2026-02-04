# Proof: Why Median Minimizes Absolute Deviations

## Problem Description (English)
Prove mathematically that for a set of points $x_1, x_2, \dots, x_n$, the value $c$ that minimizes the sum of absolute distances $f(c) = \sum_{i=1}^n |x_i - c|$ is the **median** of the set.

## Mathematical Proof

### 1. Calculus Approach (Sub-gradient)
To find the minimum of $f(c) = \sum_{i=1}^n |x_i - c|$, we take the derivative with respect to $c$ and set it to zero.

The derivative of $|x_i - c|$ with respect to $c$ is:
- $-1$ if $c < x_i$
- $+1$ if $c > x_i$
- Undefined at $c = x_i$ (we use sub-gradients here).

Summing these up:
$$f'(c) = \sum_{i=1}^n \text{sgn}(c - x_i)$$
Where $\text{sgn}(z)$ is the sign function.

For $f(c)$ to be minimized, we need $f'(c) = 0$:
- This means the number of points where $c > x_i$ must equal the number of points where $c < x_i$.
- In other words, $c$ must have an equal number of data points to its left and to its right.
- This is the definition of the **Median**.

### 2. Geometric / Intuitive Approach
Consider moving $c$ by a small distance $\Delta$ to the right:
- For every point $x_i$ to the left of $c$ ($x_i < c$), the distance $|x_i - c|$ **increases** by $\Delta$.
- For every point $x_j$ to the right of $c$ ($x_j > c$), the distance $|x_j - c|$ **decreases** by $\Delta$.

Let $N_{left}$ be the number of points to the left and $N_{right}$ be the number of points to the right.
The net change in the total distance is:
$$\text{Change} = \Delta \cdot N_{left} - \Delta \cdot N_{right} = \Delta (N_{left} - N_{right})$$

- If $N_{right} > N_{left}$, the total distance **decreases** as we move right.
- If $N_{left} > N_{right}$, the total distance **decreases** as we move left.
- The total distance is minimized only when $N_{left} = N_{right}$, which occurs when $c$ is the median.

## Comparison: Mean vs. Median
- **Median** minimizes $\sum |x_i - c|$ (L1 norm).
- **Mean** minimizes $\sum (x_i - c)^2$ (L2 norm). 
    - *Proof for Mean*: $\frac{d}{dc} \sum (x_i - c)^2 = \sum -2(x_i - c) = 0 \implies \sum x_i - nc = 0 \implies c = \bar{x}$.

## Spoken / Interview Answer
"To prove that the median minimizes the sum of absolute distances, I look at how the total distance changes when I move the point $c$. 
If I move $c$ slightly to the right, the distance to all points on the left increases, while the distance to all points on the right decreases. 
To keep the total distance from increasing, I need to have an equal number of points on both sides. If there are more points on the right, moving right will always reduce the total sum. Therefore, the minimum must be at the point where the number of elements on the left equals the number of elements on the right, which is the **Median**. 
By contrast, if we were minimizing the **squared** distances (L2), the 'pull' of each point would be proportional to its distance, leading us to the **Mean**."

