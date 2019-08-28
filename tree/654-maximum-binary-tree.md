# 654. Maximum Binary Tree
problem [link](https://leetcode.com/problems/maximum-binary-tree/)

## Brute force solution
### Intuition
We use a helper function `helper(array, start, end)` to form the tree of `array[start:end]`. Given an array, find the index of the max element `maxIdx`. The left subtree will be `helper(array, start, maxIdx - 1)`, and the right 
subtree will be `helper(array, maxIdx + 1, end)`. We recursively call these funtions to form the tree.

### Code
```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        if (nums == null) return null;
        return build(nums, 0, nums.length - 1);
    }
    
    private TreeNode build(int[] nums, int start, int end) {
        if (start > end) return null;
        
        int idxMax = start;
        for (int i = start + 1; i <= end; i++) {
            if (nums[i] > nums[idxMax]) {
                idxMax = i;
            }
        }
        
        TreeNode root = new TreeNode(nums[idxMax]);
        
        root.left = build(nums, start, idxMax - 1);
        root.right = build(nums, idxMax + 1, end);
        
        return root;
    }
}
```
### Time complexity
* O(n^2) for time complexity at worst situation, i.e. [1,2,3,4,5,6]
* O(1) for space complexity apart from the tree itself.

## Recursive solution with stack
Use a stack to keep track of the max numbers that we have encountered when locating the `max` element. Using a stack ensures that
the elements in the stack is guaranteed to be in increasing order. For the left subtree, we can keep using the stack after poping the 
last pushed element. For the right subtree, we use a new stack. When we recursively call `helper`, we can check if the stack is empty, 
if not, we can pop the last element, which is gauranteed to be the max element among `array[start:end]`.

### Code
```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        TreeNode root = helper(nums, 0, nums.length - 1, new Stack<Integer>());
        return root;
    }
    
    public TreeNode helper(int[] nums, int start, int end, Stack<Integer> s) {
        if (start == end) {
            return new TreeNode(nums[start]);
        }
        if (s.isEmpty()) {
            s.push(start);
            for (int i = start + 1; i <= end; i++) {
                if (nums[i] > nums[s.peek()]) {
                    s.push(i);
                }
            }
        }
        
        int maxIdx = s.pop();
        TreeNode node = new TreeNode(nums[maxIdx]);
        if (start <= maxIdx - 1) {
            if (!s.isEmpty() && s.peek() > maxIdx) {
                s = new Stack<Integer>();
            }
            TreeNode left = helper(nums, start, maxIdx - 1, s);
            node.left = left;
        }
        
        if (end >= maxIdx + 1) {
            if (!s.isEmpty() && s.peek() < maxIdx) {
                s = new Stack<Integer>();
            }
            TreeNode right = helper(nums, maxIdx + 1, end, s);
            node.right = right;
        }
        
        return node;
    }
}
```

### Time complexity
* O(n^2) for time complexity at worst situation, i.e. [5,4,3,2,1]. But theoretically deals with general cases better than the brute force.
* O(n) for space complexity apart from the tree.

### Comment
For the test case provided on LeetCode, this alogrithm runs much slower than the brute force recursive alogrihtm.

## One pass solution
### Intuition
sleepingpig offers a great explanation [here](https://leetcode.com/problems/maximum-binary-tree/discuss/106156/Java-worst-case-O(N)-solution).
My own understanding is that the algorithms ensures that the stack contains the order of the path to its elements before inserting `nums[i]`.
It also takes advantage of the fact that when you change the end of an edge, it wouldn't affect any node, even for the node that was
previously connected to the edge.

### Code
```java
class Solution {
    public TreeNode constructMaximumBinaryTree(int[] nums) {
        Deque<TreeNode> stack = new LinkedList<TreeNode>();
        for (int i = 0; i < nums.length; i++) {
            TreeNode cur = new TreeNode(nums[i]);
            while (!stack.isEmpty() && stack.peek().val < nums[i]) {
                cur.left = stack.pop();
            }
            
            if (!stack.isEmpty()) {
                stack.peek().right = cur;
            }
            
            stack.push(cur);
        } 
        
        return stack.isEmpty() ? null : stack.removeLast();
    }
}
```
