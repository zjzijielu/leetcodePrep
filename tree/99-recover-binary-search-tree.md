# 99. Recover Binary Search Tree
problem [link](https://leetcode.com/problems/recover-binary-search-tree/)

## Solution
### Intuition
When it comes to binary tree, in-order traversal goes through the values in an ascending order. So now the problem can be thought of 
as traversing through a list that was supposed to be sorted in asending order, but has two elements misplaced, such as follows:

`6 3 4 5 2`

We can use a `prev` pointer, and check if the current value that we are looking at is bigger than the previous value, i.e. `prev < curr`. If not, then 
the `prev` value must be the 1st misplaced number, the 2nd misplaced number might be `cur`. 
We update the 2nd misplaced number when we see again `prev > curr`.

In in-order traversal, we keep `prev` as the previous node that we are looking at, and `root` as the node that we are currently looking at.

### Code
```java
class Solution {
    TreeNode first = null;
    TreeNode second = null;
    TreeNode pre = new TreeNode(Integer.MIN_VALUE);
    
    public void recoverTree(TreeNode root) {
        traverse(root);
        
        int tmp = first.val;
        first.val = second.val;
        second.val = tmp;
    }
    
    public void traverse(TreeNode root) {
        if (root == null) {
            return;
        }
        
        traverse(root.left);
        if (first == null && pre.val > root.val) {
            first = pre;
        }
        
        if (first != null && pre.val > root.val) {
            second = root;
        }
        
        pre = root;
        
        traverse(root.right);
        
    }
}
```

### Complexity analysis
* O(n) for time complexity
* O(n) for space complexity, as we are doing recursion




