# 84. Largest Rectangle in Histogram
problem [link](https://leetcode.com/problems/largest-rectangle-in-histogram/)

## Brute Force Solution
### Intuition
The basic idea is that for each `heights[i]`, we obtain the both the leftmost and the rightmost height that is larger or equal to `heights[i]`.
Then the corresponding area is `heights[i] * (rightMost - leftMost + 1)`

### Complexity analysis
* O(n^2) for time complexity, worst case being [1,1,1,1,1,1]
* O(1) for space complexity, as we only need to keep track of `max`

## Brute Force Solution improved
### Intuition
We keep track of `lessFromLeft` and `lessFromRight` for every height. Notice that if we have recorded all `lessFromLeft[:i]`, we don't need
to traverse all the way to the left again. Instead, we can start from `i-1` and check if `heights[i-1] > heights[i]`. If so, we go back to index `lessFromLeft[i-1]`. And repeat until we either reach index 0 or `height[p] < height[i]`.
The reason why this works is that if `heights[p] <= heights[i]`, then `heights[lessFromLeft[p]]` might be less than `heights[i]`.

### Code
```java
class Solution {
    public int largestRectangleArea(int[] heights) {
        if (heights == null || heights.length == 0) {
            return 0;
        }
        int n = heights.length;
        int[] lessFromLeft = new int[n];
        int[] lessFromRight = new int[n];
        lessFromLeft[0] = -1;
        lessFromRight[n - 1] = n;
        int maxArea = 0, p = 0;
        
        for (int i = 1; i < n; i++) {
            p = i - 1;
            while (p >= 0 && heights[p] >= heights[i]) {
                p = lessFromLeft[p];
            }
            lessFromLeft[i] = p;
        }
        
        for (int i = n - 2; i >= 0; i--) {
            p = i + 1;
            while(p <= n - 1 && heights[p] >= heights[i]) {
                p = lessFromRight[p];
            }
            lessFromRight[i] = p;
        }
        
        for (int i = 0; i < n; i++) {
            maxArea = Math.max(heights[i] * (lessFromRight[i] - lessFromLeft[i] - 1), maxArea);
        }
        
        return maxArea;
    }
}
```

### Complexity analysis
* O(n) for time complexity
* O(n) for space complexity, as we store both `lessFromLeft` and `lessFromRight`
