# Sort Transformed Array

## Problem Description
Given a **sorted** array of integers `nums` and three integers `a`, `b`, and `c`, apply a quadratic function of the form $f(x) = ax^2 + bx + c$ to each element `nums[i]` in the array, and return the array in a **sorted** order.

## Approach
Since the input array is already sorted, the quadratic function $f(x) = ax^2 + bx + c$ follows a parabolic shape:
1.  **Case $a > 0$**: The parabola opens upwards. The largest values will be at the two ends of the sorted array, and the smallest value will be near the vertex ($x = -b/2a$).
2.  **Case $a < 0$**: The parabola opens downwards. The smallest values will be at the two ends, and the largest value will be near the vertex.
3.  **Case $a = 0$**: The function is linear ($f(x) = bx + c$). If $b \ge 0$, the order remains the same; if $b < 0$, the order is reversed.

**Two-Pointer Strategy ($O(n)$):**
- Use two pointers, `left` at the start and `right` at the end of the array.
- If $a > 0$, compare $f(nums[left])$ and $f(nums[right])$. Place the larger value at the **end** of the result array and move the corresponding pointer.
- If $a \le 0$, compare $f(nums[left])$ and $f(nums[right])$. Place the smaller value at the **beginning** of the result array and move the corresponding pointer.

## Code with Test Cases

```python
def sort_transformed_array(nums, a, b, c):
    def f(x):
        return a * x * x + b * x + c

    n = len(nums)
    res = [0] * n
    left, right = 0, n - 1
    
    # If a > 0, the largest values are at the ends. 
    # We fill the result from right to left.
    if a > 0:
        idx = n - 1
        while left <= right:
            val_l, val_r = f(nums[left]), f(nums[right])
            if val_l > val_r:
                res[idx] = val_l
                left += 1
            else:
                res[idx] = val_r
                right -= 1
            idx -= 1
    # If a <= 0, the smallest values are at the ends.
    # We fill the result from left to right.
    else:
        idx = 0
        while left <= right:
            val_l, val_r = f(nums[left]), f(nums[right])
            if val_l < val_r:
                res[idx] = val_l
                left += 1
            else:
                res[idx] = val_r
                right -= 1
            idx += 1
            
    return res

# --- Test Cases ---
if __name__ == "__main__":
    # Case 1: a > 0 (Upward Parabola)
    print(f"Test 1: {sort_transformed_array([-4, -2, 2, 4], 1, 3, 5)} | Expected: [3, 9, 15, 33]")
    
    # Case 2: a < 0 (Downward Parabola)
    print(f"Test 2: {sort_transformed_array([-4, -2, 2, 4], -1, 3, 5)} | Expected: [-23, -5, 1, 7]")
    
    # Case 3: a = 0 (Linear, b > 0)
    print(f"Test 3: {sort_transformed_array([-4, -2, 2, 4], 0, 3, 5)} | Expected: [-7, -1, 11, 17]")
    
    # Case 4: a = 0 (Linear, b < 0)
    print(f"Test 4: {sort_transformed_array([-4, -2, 2, 4], 0, -3, 5)} | Expected: [-7, -1, 11, 17]")
```

## Complexity Analysis
- **Time Complexity**: $O(n)$, where $n$ is the number of elements. We traverse the array exactly once with two pointers.
- **Space Complexity**: $O(n)$ to store the result array (or $O(1)$ if we don't count the output).


