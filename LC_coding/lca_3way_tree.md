# Lowest Common Ancestor in a 3-Way Tree

## Problem Description (English)
Given a 3-way tree (where each node has at most 3 children) and two nodes `p` and `q` within the tree, find their **Lowest Common Ancestor (LCA)**.

The lowest common ancestor is defined between two nodes `p` and `q` as the lowest node in $T$ that has both `p` and `q` as descendants (where we allow a node to be a descendant of itself).

## Approach
The algorithm is a generalization of the LCA algorithm for binary trees using **Depth-First Search (DFS)**.

1.  **Base Case**:
    - If the current `root` is `None`, return `None`.
    - If the current `root` is either `p` or `q`, return `root`.
2.  **Recursive Step**:
    - Recursively call the LCA function on all three children: `left`, `middle`, and `right`.
3.  **Combine Results**:
    - If **two** of the subtrees return a non-null value, it means `p` and `q` are located in different subtrees. Therefore, the current `root` is the LCA.
    - If the current `root` is `p` or `q` AND **one** of the subtrees returns a non-null value, the current `root` is the LCA.
    - If only **one** subtree returns a non-null value, return that value (the ancestor is further down).
    - If none return non-null, return `None`.

## Code with Test Cases

```python
class TernaryNode:
    def __init__(self, val):
        self.val = val
        self.children = [] # List of up to 3 children

def find_lca_3way(root, p, q):
    if not root or root == p or root == q:
        return root
    
    found_in_children = []
    for child in root.children:
        res = find_lca_3way(child, p, q)
        if res:
            found_in_children.append(res)
            
    # If two children returned non-None, this root is the LCA
    if len(found_in_children) == 2:
        return root
    
    # If only one child returned non-None, pass it up
    return found_in_children[0] if len(found_in_children) == 1 else None

# --- Test Cases ---
if __name__ == "__main__":
    # Constructing a 3-way tree:
    #        1
    #     /  |  \
    #    2   3   4
    #   /|\      |
    #  5 6 7     8
    
    root = TernaryNode(1)
    n2 = TernaryNode(2)
    n3 = TernaryNode(3)
    n4 = TernaryNode(4)
    root.children = [n2, n3, n4]
    
    n5 = TernaryNode(5)
    n6 = TernaryNode(6)
    n7 = TernaryNode(7)
    n2.children = [n5, n6, n7]
    
    n8 = TernaryNode(8)
    n4.children = [n8]

    # Test 1: LCA of 5 and 7 (Both under 2)
    lca1 = find_lca_3way(root, n5, n7)
    print(f"LCA of 5 and 7: {lca1.val} | Expected: 2")

    # Test 2: LCA of 5 and 3 (Under different main branches)
    lca2 = find_lca_3way(root, n5, n3)
    print(f"LCA of 5 and 3: {lca2.val} | Expected: 1")

    # Test 3: LCA of 2 and 6 (2 is parent of 6)
    lca3 = find_lca_3way(root, n2, n6)
    print(f"LCA of 2 and 6: {lca3.val} | Expected: 2")

    # Test 4: LCA of 8 and 3
    lca4 = find_lca_3way(root, n8, n3)
    print(f"LCA of 8 and 3: {lca4.val} | Expected: 1")
```

## Complexity Analysis
- **Time Complexity**: $O(N)$, where $N$ is the number of nodes in the tree. In the worst case, we visit every node once.
- **Space Complexity**: $O(H)$, where $H$ is the height of the tree. This is due to the recursion stack. For a balanced tree, $H = O(\log_3 N)$; for a skewed tree, $H = O(N)$.


