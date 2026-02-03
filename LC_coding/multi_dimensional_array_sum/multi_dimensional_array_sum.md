# Multi-dimensional Array Sum

## Problem Description
You are given a multi-dimensional array with dimensions $n_1 \times n_2 \times n_3 \times \dots \times n_x$, where $x$ can be any arbitrary integer. The only way to access the elements is through a `get(index)` method, where `index` is a list or tuple of coordinates (e.g., `[i1, i2, ..., ix]`). 

Calculate and return the sum of all elements in the array.

## Approach
Since the number of dimensions $x$ is unknown and can vary, we use **Recursion (Depth-First Search)** to traverse all possible indices.

1.  **State**: The recursive function will maintain the `current_index` (a list of coordinates being built).
2.  **Base Case**: When the length of `current_index` equals the number of dimensions $x$, we have a complete coordinate. Call `get(current_index)` and return the value.
3.  **Recursive Step**: 
    - Determine which dimension we are currently processing (e.g., `dim_idx = len(current_index)`).
    - Loop through all possible values for this dimension, from `0` to `n_{dim_idx} - 1`.
    - Append the loop variable to `current_index`, recurse to the next dimension, and add the result to a running sum.
    - Backtrack by removing the last coordinate added.

## Code with Test Cases

```python
class MultiDimensionalArray:
    """Mock class to simulate the multi-dimensional array"""
    def __init__(self, dimensions, data_dict):
        self.dimensions = dimensions
        self.data = data_dict # Simulation: {(0,0,0): 1, (0,0,1): 2, ...}

    def get(self, index):
        return self.data.get(tuple(index), 0)

def sum_multi_dimensional_array(md_array):
    dimensions = md_array.dimensions
    x = len(dimensions)
    
    def dfs(current_index):
        # Base case: we have a full coordinate for all x dimensions
        if len(current_index) == x:
            return md_array.get(current_index)
        
        total = 0
        dim_to_process = len(current_index)
        # Iterate through the size of the current dimension
        for i in range(dimensions[dim_to_process]):
            current_index.append(i)
            total += dfs(current_index)
            current_index.pop() # Backtrack
            
        return total

    return dfs([])

# --- Test Cases ---
if __name__ == "__main__":
    # Test Case 1: 2x2 array
    # [[1, 2], [3, 4]] -> sum = 10
    dimensions1 = [2, 2]
    data1 = {(0,0): 1, (0,1): 2, (1,0): 3, (1,1): 4}
    arr1 = MultiDimensionalArray(dimensions1, data1)
    print(f"Test 1 Sum: {sum_multi_dimensional_array(arr1)} | Expected: 10")
    
    # Test Case 2: 2x3x2 array
    # Sum of all elements is 1*12 = 12 if all elements are 1
    dimensions2 = [2, 3, 2]
    data2 = {}
    total_expected = 0
    import random
    for i in range(2):
        for j in range(3):
            for k in range(2):
                val = random.randint(1, 10)
                data2[(i,j,k)] = val
                total_expected += val
    arr2 = MultiDimensionalArray(dimensions2, data2)
    print(f"Test 2 Sum: {sum_multi_dimensional_array(arr2)} | Expected: {total_expected}")

    # Test Case 3: 1D array
    dimensions3 = [5]
    data3 = {(0,):1, (1,):1, (2,):1, (3,):1, (4,):1}
    arr3 = MultiDimensionalArray(dimensions3, data3)
    print(f"Test 3 Sum: {sum_multi_dimensional_array(arr3)} | Expected: 5")
```

## Complexity Analysis
- **Time Complexity**: $O(N)$, where $N = \prod_{i=1}^{x} n_i$ is the total number of elements in the array. We visit every single element exactly once.
- **Space Complexity**: $O(x)$, where $x$ is the number of dimensions. This is due to the recursion stack depth and the `current_index` list, both of which are proportional to the number of dimensions.

