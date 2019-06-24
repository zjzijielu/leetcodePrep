# 108. Convert Sorted Array to Binary Search Tree
problem [link](https://leetcode.com/problems/convert-sorted-array-to-binary-search-tree/)

## Solution
### Intuition
In-order traversal [left, root, right] of a bst produces a sorted array. As a result, the middle value is the root.
Reverse engineering will give us the result
```java
class Solution {
    public TreeNode sortedArrayToBST(int[] nums) {
        int n = nums.length;
        if (n == 0) {
            return null;
        }
        
        return helper(nums, 0, n - 1);
        
    }
    
    public TreeNode helper(int[] nums, int low, int high) {
        if (low > high) { 
            return null;
        }
        int mid = low + (high - low) / 2;
        TreeNode node = new TreeNode(nums[mid]);
        node.left = helper(nums, low, mid - 1);
        node.right = helper(nums, mid + 1, high);
        
        return node;
    }
}
```

## Complexity analysis
* O(n) for time complexity as we are only traversing the array once.
* O(n) for space complexity as we only need n nodes for every element.

