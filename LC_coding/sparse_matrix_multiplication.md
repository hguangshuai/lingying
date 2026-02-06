# Efficient Sparse Vector and Matrix Multiplication

## Problem Description
Design and implement classes for **Sparse Vector** and **Sparse Matrix** from scratch. 
- **Memory Constraint**: Must be optimized for sparse data (only store non-zero elements).
- **Time Complexity**: Multiplication must be more efficient than $O(M \times N)$ or $O(M \times N \times K)$. It should only depend on the number of non-zero elements (**nnz**).

## Approach

### 1. Data Structure Selection
To optimize for both memory and row-based iteration (crucial for matrix multiplication), we use a **Dictionary of Dictionaries** (or a Hash Map of Hash Maps).
- **Sparse Vector**: `self.data = {index: value}`
- **Sparse Matrix**: `self.data = {row_index: {col_index: value}}`

### 2. Dot Product (Sparse Vectors)
To compute the dot product of two sparse vectors $A$ and $B$:
- Iterate over the smaller dictionary to minimize lookups.
- For each index $i$ in $A$, if $i$ also exists in $B$, add $A[i] \times B[i]$ to the total.
- **Complexity**: $O(\min(\text{nnz}_A, \text{nnz}_B))$.

### 3. Matrix Multiplication (Sparse Matrices)
To compute $C = A \times B$:
- Standard multiplication is $O(M \times K \times N)$.
- **Optimized Strategy**: 
    1. Iterate through non-zero elements of $A$: for each $A[i][k] = v_a$.
    2. Directly look up row $k$ in matrix $B$.
    3. For each non-zero element $B[k][j] = v_b$, update the result: $C[i][j] += v_a \times v_b$.
- **Complexity**: $O(\sum_{i, k: A_{ik} \neq 0} \text{count\_nonzero}(B_{k, \cdot}))$. This skips all zeros in both $A$ and $B$.

## Code Implementation

```python
class SparseVector:
    def __init__(self, nums):
        # Memory optimization: only store non-zero values
        self.data = {i: val for i, val in enumerate(nums) if val != 0}
        self.size = len(nums)

    def dot_product(self, other):
        if self.size != other.size:
            raise ValueError("Vector dimensions must match")
        
        # Performance optimization: iterate over the shorter dictionary
        if len(self.data) > len(other.data):
            return other.dot_product(self)
        
        total = 0
        for idx, val in self.data.items():
            if idx in other.data:
                total += val * other.data[idx]
        return total

class SparseMatrix:
    def __init__(self, matrix):
        self.rows = len(matrix)
        self.cols = len(matrix[0]) if self.rows > 0 else 0
        # data[i][j] = value
        self.data = {}
        for i in range(self.rows):
            for j in range(self.cols):
                if matrix[i][j] != 0:
                    if i not in self.data:
                        self.data[i] = {}
                    self.data[i][j] = matrix[i][j]

    def multiply(self, other):
        if self.cols != other.rows:
            raise ValueError("Matrix dimensions incompatible for multiplication")
        
        # Resulting matrix dimensions: self.rows x other.cols
        result = {} # Using a dict for result to keep it sparse during calculation
        
        # Optimized Sparse Matrix Multiplication
        # For each non-zero element A[i][k]
        for i, row_a in self.data.items():
            for k, val_a in row_a.items():
                # If matrix B has any non-zero elements in row k
                if k in other.data:
                    if i not in result:
                        result[i] = {}
                    for j, val_b in other.data[k].items():
                        # C[i][j] += A[i][k] * B[k][j]
                        result[i][j] = result[i].get(j, 0) + val_a * val_b
        
        return self._to_dense(result, self.rows, other.cols)

    def _to_dense(self, sparse_dict, r, c):
        # Convert the resulting sparse dict back to a dense list of lists for easy viewing
        dense = [[0] * c for _ in range(r)]
        for i, row in sparse_dict.items():
            for j, val in row.items():
                dense[i][j] = val
        return dense

# --- Test Cases ---
if __name__ == "__main__":
    # 1. Sparse Vector Dot Product
    v1 = SparseVector([1, 0, 0, 2, 3])
    v2 = SparseVector([0, 3, 0, 4, 0])
    print(f"Vector Dot Product: {v1.dot_product(v2)} | Expected: 8") # 2*4

    # 2. Sparse Matrix Multiplication
    # A = [[1, 0, 0], [-1, 0, 3]] (2x3)
    # B = [[7, 0], [0, 0], [0, 1]] (3x2)
    # Result = A * B = [[7, 0], [-7, 3]] (2x2)
    mat_a = SparseMatrix([[1, 0, 0], [-1, 0, 3]])
    mat_b = SparseMatrix([[7, 0], [0, 0], [0, 1]])
    
    result = mat_a.multiply(mat_b)
    print(f"Matrix Multiplication Result: {result}")
    assert result == [[7, 0], [-7, 3]]
    print("Test Passed!")
```

## Complexity Analysis
- **Memory**: $O(\text{nnz})$, where **nnz** is the number of non-zero elements.
- **Time (Dot Product)**: $O(\min(\text{nnz}_A, \text{nnz}_B))$.
- **Time (Matrix Multiplication)**: $O(\text{nnz}_A \times \text{avg\_nnz\_per\_row}_B)$. This is far superior to $O(M \times K \times N)$ when the matrices are sparse.


