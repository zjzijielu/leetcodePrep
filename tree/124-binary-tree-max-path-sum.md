# 124. Binary Tree Maximum Path Sum
problem [link](https://leetcode.com/problems/binary-tree-maximum-path-sum/)

## Wrong Solution
### Code 
```java
class Solution {
    public int maxPathSum(TreeNode root) {
        return helper(root);
    }
    
    public int helper(TreeNode root) {
        if (root.left == null && root.right == null) {
            return root.val;
        }
        
        int max = Integer.MIN_VALUE;
        int leftMax = 0, rightMax = 0;
        if (root.left != null) {
            // get left max path
            leftMax = helper(root.left);
            max = leftMax;
        }
        if (root.right != null) {
            // get right max path
            rightMax = helper(root.right);
            max = Math.max(max, rightMax);
        }
        
        if (max < 0) {
            max = Math.max(max, root.val);     
        } else {
            max = Math.max(max, max + root.val);    
        }
        
        if (root.left != null && root.right != null) {
            max = Math.max(max, leftMax + root.val + rightMax);    
        }
        return max;
    }
}
```
### Comment
The following line
```java
max = Math.max(max, leftMax + root.val + rightMax);    
```
is problematic because it might return a spanning tree-like graph instead of a path.

## Solution
### Intuition
The tricky part about this problem is the requirement of finding a path. Our path here is always a subtree structure. We cannot include a node with both its left and right children
unless its the root of the subtree path.

### Code
```java
class Solution {
    int maxValue;
    
    public int maxPathSum(TreeNode root) {
        maxValue = Integer.MIN_VALUE;
        helper(root);
        return maxValue;
    }
    
    public int helper(TreeNode root) {
        if (root == null) {
            return 0;
        }
    
        int left = Math.max(0, helper(root.left));
        int right = Math.max(0, helper(root.right));
        maxValue = Math.max(maxValue, left + root.val + right);
        return Math.max(left, right) + root.val;
    }
}
```
