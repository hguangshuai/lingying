# Gini Coefficient (Income Inequality)

## Problem Description (English)
Implement the **Gini Coefficient**, a measure of statistical dispersion intended to represent the income inequality or wealth inequality within a nation or any other group of people. 

**Note**: This is different from the *Gini Impurity* used in decision trees.
- A Gini coefficient of **0** expresses perfect equality (everyone has the same income).
- A Gini coefficient of **1** (or 100%) expresses maximal inequality (one person has all the income).

## Approach
For a discrete set of data $x$ (income of $n$ individuals), the Gini coefficient can be calculated efficiently if the data is sorted ($x_1 \le x_2 \le \dots \le x_n$):

$$G = \frac{\sum_{i=1}^n (2i - n - 1) x_i}{n \sum_{i=1}^n x_i}$$

Where:
- $n$ is the number of individuals.
- $x_i$ is the income of the $i$-th individual (after sorting).
- $i$ is the rank (from 1 to $n$).

### Steps:
1.  Sort the input list of incomes in non-decreasing order.
2.  Calculate the sum of all incomes.
3.  Apply the formula using the index (rank) of each element.

## Code with Test Cases

```python
def calculate_gini(incomes):
    """
    Calculates the Gini coefficient of a list of incomes.
    """
    if not incomes:
        return 0
    
    # 1. Sort the incomes
    data = sorted(incomes)
    n = len(data)
    
    # 2. Calculate the denominator (n * sum of incomes)
    total_income = sum(data)
    if total_income == 0:
        return 0 # Avoid division by zero
        
    # 3. Calculate the numerator using the sorted formula
    # Sum of (2*i - n - 1) * x_i
    # Note: i is 1-indexed in the formula, so we use (j + 1)
    weighted_sum = 0
    for j in range(n):
        rank = j + 1
        weighted_sum += (2 * rank - n - 1) * data[j]
        
    return weighted_sum / (n * total_income)

# --- Test Cases ---
if __name__ == "__main__":
    # Case 1: Perfect equality (Everyone earns 100)
    print(f"Perfect Equality: {calculate_gini([100, 100, 100, 100, 100])} | Expected: 0.0")
    
    # Case 2: Extreme inequality (One person has everything)
    # With 5 people, the max Gini is (n-1)/n = 4/5 = 0.8
    print(f"Extreme Inequality: {calculate_gini([0, 0, 0, 0, 100])} | Expected: 0.8")
    
    # Case 3: Mixed incomes
    incomes = [10, 20, 30, 40, 50]
    print(f"Mixed Incomes {incomes}: {calculate_gini(incomes):.4f}")
    
    # Case 4: Real-world like scenario
    # A few high earners, many low earners
    incomes2 = [1000, 200, 150, 100, 50]
    print(f"Large Gap {incomes2}: {calculate_gini(incomes2):.4f}")
```

## Complexity Analysis
- **Time Complexity**: $O(n \log n)$ due to the sorting step. The subsequent summation is $O(n)$.
- **Space Complexity**: $O(n)$ if we create a new sorted list, or $O(1)$ if we sort in-place (depending on Python's `sorted` implementation/usage).


