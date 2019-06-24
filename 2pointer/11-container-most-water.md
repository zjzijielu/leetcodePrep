# 11. Container With Most Water
problem [link](https://leetcode.com/problems/container-with-most-water/)

## Solution
### Intuition
The volume of the container is determined by the shorter line. We use two pointers to get two lines.
So after we check the area of a specific container, we should change the shorter line, because
even if we change the longer line, the volume will either decrease or remain the same as we are moving inward.
However, if we move the shorter line, we might get a volume that is bigger than the previous one.

### Code
This code is different from the solution code
```java
class Solution {
    public int maxArea(int[] height) {
        int n = height.length;
        if (n <= 1) {
            return 0;
        }
        
        int i = 0;
        int j = n - 1;
        int maxArea = 0;
        int smaller = 0;
        
        while (i < j) {
            if (height[i] < height[j]) {
                smaller = height[i];
                i++;
            } else {
                smaller = height[j];
                j--;
            }
            if (smaller * (j - i + 1) > maxArea) {
                maxArea = smaller * (j - i + 1);
            }
        }
        
        return maxArea;
        
    }
}
```
### Complexity analysis
* O(n) for time complexity, as we will only traverse the array once
* O(1) for space complexity, as we only need to store two pointers and the maximum area value.

## Comment
Given an array, and if we are to deal with two values in the array, we probably want to use two pointers (?)
