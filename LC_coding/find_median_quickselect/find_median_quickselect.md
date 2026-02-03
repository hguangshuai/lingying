# Find Median (Quickselect)

## Problem Description
Given an unsorted array of integers `nums`, find its median using the **Quickselect** algorithm.

The median is the middle value in an ordered integer list.
- If the size of the list is odd, the median is the middle element.
- If the size of the list is even, the median is the average of the two middle elements.

## Approach
Quickselect is an optimization of Quicksort used to find the $k$-th smallest element in an unordered list. Instead of recursing into both halves, we only recurse into the partition that contains the target index.

1.  **Define a `quickselect` helper**: This function takes a list and a target index `k`.
2.  **Partitioning**: Pick a pivot and partition the array such that elements smaller than the pivot are on the left and larger are on the right.
3.  **Recursive Step**:
    - If the pivot's position is exactly `k`, we found the element.
    - If the pivot's position is greater than `k`, search the left part.
    - If the pivot's position is smaller than `k`, search the right part.
4.  **Handling Even Length**: For an array of length $n$, if $n$ is even, we need the $(n/2-1)$-th and $(n/2)$-th elements and take their average. If $n$ is odd, we just need the $(n//2)$-th element.

## Code with Test Cases

```python
import random

def find_median(nums):
    if not nums:
        return 0
    
    n = len(nums)
    
    def quickselect(left, right, k):
        """Find the k-th smallest element (0-indexed) in nums[left:right+1]"""
        if left == right:
            return nums[left]
        
        # Random pivot selection
        pivot_idx = random.randint(left, right)
        nums[pivot_idx], nums[right] = nums[right], nums[pivot_idx]
        
        # Partition
        pivot = nums[right]
        i = left
        for j in range(left, right):
            if nums[j] < pivot:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
        nums[i], nums[right] = nums[right], nums[i]
        
        if i == k:
            return nums[i]
        elif i > k:
            return quickselect(left, i - 1, k)
        else:
            return quickselect(i + 1, right, k)

    if n % 2 == 1:
        return float(quickselect(0, n - 1, n // 2))
    else:
        left_mid = quickselect(0, n - 1, n // 2 - 1)
        # Note: After the first quickselect, nums might be partially reordered.
        # We need to find the second mid in the correct range.
        right_mid = quickselect(0, n - 1, n // 2)
        return (left_mid + right_mid) / 2.0

# --- Test Cases ---
if __name__ == "__main__":
    test_cases = [
        ([3, 2, 1, 5, 4], 3.0),
        ([3, 2, 3, 1, 2, 4, 5, 5, 6], 3.0),
        ([1, 2], 1.5),
        ([1], 1.0),
        ([7, 10, 4, 3, 20, 15], 12.5)
    ]
    
    for nums, expected in test_cases:
        # We copy nums because find_median modifies the list in-place
        res = find_median(nums.copy())
        print(f"Input: {nums} | Expected: {expected} | Result: {res}")
        assert res == expected
```

## Complexity Analysis
- **Time Complexity**:
    - **Average Case**: $O(n)$, where $n$ is the number of elements. Each partition step takes linear time, and on average, we discard half of the array.
    - **Worst Case**: $O(n^2)$, if the pivot selection is consistently poor (e.g., already sorted array and picking the last element). Random pivot selection makes this extremely unlikely.
- **Space Complexity**:
    - **Average Case**: $O(\log n)$ due to the recursion stack.
    - **Worst Case**: $O(n)$ for the recursion stack in the worst partitioning scenario.
    - **In-place**: The algorithm modifies the input array in-place, requiring $O(1)$ additional storage excluding recursion.

