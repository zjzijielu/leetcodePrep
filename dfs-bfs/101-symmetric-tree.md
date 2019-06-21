# 101. Symmetric Tree
problem [link](https://leetcode.com/problems/symmetric-tree/)

## Solution
### Intuition
To check whether one tree is the mirror of another, we do one BFS on tree1, and we do BFS on tree2 in an reverse order.
I use two queues to do BFS, and whenever we pop from both queues, we check whether the values are the same.

```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Queue<TreeNode> q1 = new LinkedList();
        Queue<TreeNode> q2 = new LinkedList();
        
        q1.add(root.left);
        q2.add(root.right);
        
        while(q1.size() != 0 && q2.size() != 0) {
            TreeNode node1 = q1.remove();
            TreeNode node2 = q2.remove();
            
            if ((node1 == null && node2 != null) || (node1 != null && node2 == null)) {
                return false;
            } else if (node1 != null && node2 != null) {
                if (node1.val != node2.val) {
                    return false;
                } else {
                    q1.add(node1.left);
                    q1.add(node1.right);
                    q2.add(node2.right);
                    q2.add(node2.left);
                }
            }
        }
        return true;
    }
}
```

## Better solution
### Intuition
Notice that in my own solution, the iterative part with two queues part is essentially the same as having only 1 queue, 
and add elements that we will compair together consecutively into the queue. 

### Code
```java
class Solution {
    public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        
        Queue<TreeNode> q = new LinkedList();
        q.add(root.left);
        q.add(root.right);
        
        while(q.size() != 0) {
            TreeNode node1 = q.remove();
            TreeNode node2 = q.remove();
            if ((node1 == null && node2 != null) || (node1 != null && node2 == null)) {
                return false;
            } else if (node1 != null && node2 != null) {
                if (node1.val != node2.val) {
                    return false;
                }
                q.add(node1.left);
                q.add(node2.right);
                q.add(node1.right);
                q.add(node2.left);
            }
        }
        return true;
    }
}
```
### Complexity Analysis
* O(n) for time complexity, as in the worst case we will traverse through every node in the tree.
* O(n) for space complexity - BFS space complexity
