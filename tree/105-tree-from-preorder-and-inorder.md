# 105. Construct Binary Tree from Preorder and Inorder Traversal
problem [link](https://leetcode.com/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)

## Solution
### Intuition
The 1st element of preorder traversal is the root of the tree, say 5. Then we look for 5 in inorder traversal, say inorder[4] = 5.
Then inorder[0] ~ inorder[3] is the left subtree of the root. inorder[5] ~ inorder[n - 1] is the right subtree.

Now the key point is that, preorder[1] ~ preorder[4] will have the same set of elements as inorder[0] ~ inorder[3]. 
Similarly, preorder[5] ~ preorder[n - 1] will share the same set of elements as inorder[5] ~ inorder[n - 1].

Then we do this process recursively.

### Code 
```java
class Solution {
    public int[] preorderList;
    public int[] inorderList;
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        preorderList = preorder;
        inorderList = inorder;
        return helper(0, 0, preorder.length - 1);
    }
    
    public TreeNode helper(int preStart, int inStart, int inEnd) {
        if (preStart >= preorderList.length || inStart > inEnd) {
            return null;
        }
        
        TreeNode root = new TreeNode(preorderList[preStart]);
        
        int i = 0;
        for (i = inStart; i <= inEnd; i++) {
            if (inorderList[i] == root.val) {
                break;
            }
        }
        
        root.left = helper(preStart + 1, inStart, i - 1);
        root.right = helper(preStart + i - inStart + 1, i + 1, inEnd);
        
        return root;
    }
}
```

## "Better" Solution
### Improvement
In helper function, look for root.val takes O(n). However, if we use a HashMap to record all elements and their corresponding positions in inorder traversal, 
then position lookup only takes O(1).

### Code
```java
class Solution {
    public int[] preorderList;
    public int[] inorderList;
    public HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();
    
    public TreeNode buildTree(int[] preorder, int[] inorder) {
        for (int i = 0; i < preorder.length; i++) {
            map.put(inorder[i], i);
        }
        
        preorderList = preorder;
        inorderList = inorder;
        return helper(0, 0, preorder.length - 1);
    }
    
    public TreeNode helper(int preStart, int inStart, int inEnd) {
        if (preStart >= preorderList.length || inStart > inEnd) {
            return null;
        }
        
        TreeNode root = new TreeNode(preorderList[preStart]);
        
        int i = map.get(root.val);
        
        root.left = helper(preStart + 1, inStart, i - 1);
        root.right = helper(preStart + i - inStart + 1, i + 1, inEnd);
        
        return root;
    }
}
```

### Complexity Analysis
* O(n) for time complexity, we still need to go through the entire list to reconstruct the trees.
* O(n) for space complexity, if we use HashMap. Otherwise O(1) for extra memory space.

### Comment
* Use the hashmap to record the position is a trade off between speed and memory usage.
* Define global variables to reduce passing lists as arguments in recursion.
