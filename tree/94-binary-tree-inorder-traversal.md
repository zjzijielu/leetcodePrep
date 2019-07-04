# 94. Binary Tree Inorder Traversal
problem [link](https://leetcode.com/problems/binary-tree-inorder-traversal/)

## Comment
I have encountered 2 other questions that requires using inorder traversal. So this note is for summing up how to do inorder traversal,

## Recursive solution
### Code 
```java
class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List <Integer> res = new ArrayList<Integer> ();
        helper(root, res);
        return res;
    }

    public void helper(TreeNode root, List<Integer> res) {
        if (root != null) {
            if (root.left != null) {
                helper(root.left, res);
            }
            res.add(root.val);
            if (root.right != null) {
                helper(root.right, res);
            }
        }
    }
}
```

## Iterative solution
### Code
```java
public class Solution {
    public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<Integer> ();
        Stack<TreeNode> stack = new Stack<Integer> ();
        TreeNode curr = root;
        while (curr != null || !stack.isEmpty()) {
            while (curr != null) {
                stack.push(curr);
                curr = curr.left;
            }
            curr = stack.pop();
            res.add(curr.val);
            curr = curr.right;
        }
        return res;
    }
}
```
