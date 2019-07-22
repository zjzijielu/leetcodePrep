# 42. Trapping Rain Water
problem [link](https://leetcode.com/problems/trapping-rain-water/)

## Stack solution
### Intuition
The solution is very similar to that of biggest rectangle in histogram (which I have been struggling with for a long time). Before anything,
we should figure out how to get an area of trapped water. The water will be trapped when both left bar and right bar are higher than
the current bar. Suppose `height[l]` denotes left bar height and `height[r]` denotes right bar height, and `height[bottom]` denotes the
lowest bar height, then the area of water is given by 

`(min(height[l], height[r]) - height[bottom]) * (l - r - 1)`

At first glance, we are computing the area of a rectangular no matter what which is counter intuitive. But essentially, by using 
a stack store all the height values, we can ensure we are always computing square-like area.

### Code
```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        if (n == 0) {
            return 0;
        }
        
        Stack<Integer> s = new Stack<Integer>();
        int water = 0, i = 0;
        
        while (i < n) {
            while (!s.isEmpty() && height[i] > height[s.peek()]) {
                int bottom = s.pop();
                if (s.isEmpty()) break;
                int distance = i - s.peek() - 1;
                int boundedHeight = Math.min(height[i], height[s.peek()]) - height[bottom];
                water += distance * boundedHeight;
            }
            s.push(i++);
        }
        
        return water;
    }
}
```

### Complexity analysis
* O(n) for time complexity
* O(n) for space complexity

## DP solution
### Intuition
Same intuition as before. The amount of water that can be saved at a specific index `i` is determined by the minimum of its `maxLeft` and `maxRight`. 
And the water trapped will be `min(maxLeft, maxRight) - height[i]`

We do the same thing for every index `i`.

### Code
```java
class Solution {
    public int trap(int[] height) {
        int n = height.length;
        int[] maxLeft = new int[n];
        int[] maxRight = new int[n];
        
        int max = 0;
        for (int i = 0; i < n; i++) {
            if (height[i] > max) {
                max = height[i];
            }
            maxLeft[i] = max;
        }
        
        max = 0;
        for (int i = n - 1; i >= 0; i--) {
            if (height[i] > max) {
                max = height[i];
            }
            maxRight[i] = max;
        }
        
        int water = 0;
        for (int i = 0; i < n; i++) {
            water += Math.min(maxLeft[i], maxRight[i]) - height[i];
        }
        
        return water;
    }
}
```
