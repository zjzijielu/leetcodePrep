# 173. Binary Search Tree Iterator
problem [link](https://leetcode.com/problems/binary-search-tree-iterator/)

## Space O(N) Solution
### Intuition
The intuition is pretty straight-forward. We flatten the binary search tree to store everything in a list first. 
We also keep a `cur` index. When `next` is called, we return `list[cur]` and increment `cur`. When `hasNext` is called we simply check
if `cur` is out of bound.

### Space complexity
* O(1) for time complexity both `next` and `hasNext` method
* O(N) for space complexity

## Space O(h) solution
### Intuition
The previous solution is great in terms of time complexity. However, the tree might get too big, which might result in an oversized
list to store all elements. An improvement is to store nodes along a path, which is at most of height `h` (the maximal depth of a tree),
and that is already a huge improvement from O(N), since O(N) = O(2^h).

We still need to do in-order traversal since we are iterating the tree from the smallest to the biggest. If we are starting from
`root`, the first element to return upon calling `next` is the leftmost node in the. Since our recursion is not done at once, so we
need to trace back to the parent node. That's where the stack comes in. When we traverse to the leftmost node, we add all the nodes along the way.

After poping a node, we need to check if the parent node has right child. If it does, we traverse again to the leftmost tree and
add all the nodes along the way.

### Code
```java
class BSTIterator {
    TreeNode cur = new TreeNode(0);
    Stack<TreeNode> s = new Stack<TreeNode>();
    public BSTIterator(TreeNode root) {
        helper(root);
    }
    
    public void helper(TreeNode root) {
        if (root == null) {
            return;
        }
        s.add(root);
        helper(root.left);
        
    }
    
    /** @return the next smallest number */
    public int next() {
        TreeNode node = s.pop();
        helper(node.right);
        return node.val;
    }
    
    /** @return whether we have a next smallest number */
    public boolean hasNext() {
        return !s.isEmpty();
    }
}
```

### Complexity analysis
* Average O(1) time complexity
* O(h) for space complextiy

