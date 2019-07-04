# 114. Flatten Binary Tree to Linked List
problem [link](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)

## Solution
### Intuition
Flattening the tree can be done in a recursive way. Given root node `root` The recursion works as follows:
```java
/*
* flatten the left subtree L
* flatten the right subtree R
* root.left = null;
* L.right = R
* root.left = L
*/
```

### My code
```java
class Solution {
    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        helper(root);
    }
    
    public TreeNode helper(TreeNode node) {
        if (node.left == null && node.right == null) {
            return node;
        }
        
        TreeNode buff = node.right;
        TreeNode leftLast = null;
        if (node.left != null) {
            leftLast = helper(node.left);
            node.right = node.left;
            node.left = null;
            leftLast.right = buff;
        }
        
        if (buff != null) {
            TreeNode rightLast = helper(buff);
            return rightLast;
        }
        
        return leftLast;
    }
}
```

### Clearer version
Without notes the code might be a bit hard to understand, the following is a clearer version
```java
class Solution {
    public void flatten(TreeNode root) {
        if (null == root)
            return;
        
        flattenRecur(root);
    }
    
    private TreeNode flattenRecur(TreeNode root) {
        if (root.left == null && root.right == null)
            return root;
        else if (root.left == null) {
            // if root.left is empty, only needs to flatten right subtree
            return flattenRecur(root.right);
        } else if (root.right == null) {
            // if root.right is empty, only needs to flatten left subtree
            TreeNode left = flattenRecur(root.left);
            root.right = root.left;
            root.left = null;
            return left;
        } else {
            // if both are not empty, flatten left and right then stack the left
            // subtree on the right subtree
            TreeNode right = flattenRecur(root.right);
            TreeNode left = flattenRecur(root.left);
            left.right = root.right;
            root.right = root.left;
            root.left = null;
            return right;
        }
    }
}
```

### Complexity analysis
* O(n) for time complexity as we only visit each node once
* O(1) for space complexity as all operations are in-place.
