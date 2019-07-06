# 116. Populating Next Right Pointers in Each Node
problem [link](https://leetcode.com/problems/populating-next-right-pointers-in-each-node/)

## Preliminary Solution
### Intuition
Use breadth-first search. Use a queue to store all the nodes in the same level, and fix the next pointers.

### Complexity analysis
* O(n) for time complexity, since we only traverse the tree once
* O(n/2) for space complexity, since the lowest level has n/2 nodes that we have to put in the queue

## O(1) recursive solution
### Intuition
Every time, we see two nodes with subtrees, we fix the next pointers among the subtrees

### Code
```java
class Solution {
    public Node connect(Node root) {
        // do breadth first search
        if (root == null || root.left == null) {
            return root;
        }
        helper(root.left, root.right);
        return root;
    }
    
    public void helper(Node root1, Node root2) {
        root1.next = root2;
        if (root1.left == null) {
            return;
        }
        helper(root1.left, root1.right);
        helper(root1.right, root2.left);
        helper(root2.left, root2.right);
    }
}
```

### Complexity analysis
* O(n) for time complexity, however because the 3 helper functions are recursively used, essentially, we are visiting the some nodes twice.
* O(1) for space complexity

## O(1) iterative solution
### Intuition
The previous two solutions did not really take advantage of the next pointer. The logic is that `cur.next != null` and `cur.right != null`,
we can simply do `cur.right.next = cur.next.left` to save the extra `helper(root1.left, root2.left)` that we did in the recursive part.

### Code
```java
class Solution {
    public Node connect(Node root) {
        if (root == null || root.left == null) {
            return root;
        }
        
        Node levelStart = root;
        while (levelStart != null) {
            Node cur = levelStart;
            while (cur != null) {
                if (cur.left != null) {
                    cur.left.next = cur.right;
                }
                if (cur.right != null && cur.next != null) {
                    cur.right.next = cur.next.left;
                }
                cur = cur.next;
            }
            levelStart = levelStart.left;
        }
        return root;
    }
}
```

### Complexity analysis
* O(n) for time complexity
* O(1) for space complexity
