# 162. Find Peak Element
problem [link](https://leetcode.com/problems/find-peak-element/)

## Solution
### Intuition
The easiest way to solve this problem is through a linear scan. But since the problem requirement is logarithmic, so 
the solution probably is binary search. So now the problem is how we can convert it into a binary search problem.

Consider the derivative of a piecewise linear function, which is exactly our case. The way we can find a peak value is that 
if `nums[i]` has positive derivative on the left, and negative derivative on the right, then `nums[i]` is a peak value. 

Since the problem already states that `nums[-1] = nums[n]` set to negative infinity, that means the derivative at `nums[0]` is positive,
while derivative at `nums[n-1]` is negative, and this already gaurantees that there must be at least one peak element. 
Now we use binary search to locate the element that has positive derivative on the left, and negative derivative on the right.

### Code
```java
class Solution {
    public int findPeakElement(int[] nums) {
        int low = 0;
        int high = nums.length - 1;
        
        while (low < high) {
            int mid1 = (low + high) / 2;
            int mid2 = mid1 + 1;
            
            if (nums[mid1] < nums[mid2]) {
                low = mid2;
            } else {
                high = mid1;
            }
        }
        
        return low; 
    }
}
```
### Complexity analysis
* O(logn) for time complexity, as we are using binary search
* O(1) for space complexity
