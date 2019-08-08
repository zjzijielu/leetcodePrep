# 219. Contains Duplicate II
problem [link](https://leetcode.com/problems/contains-duplicate-ii/)

## Solution 1
### Intuition
We use a hashmap to record the index of the latest occurrence of a number. Next time we meet the same number, we check if its index `i` 
satisfies `i - map.get(num) <= k`. If so, we return true. Otherwise, we update the latest index with `i`. The reason is that if index `i`
is `k` larger than the last index, there's no need to check if the next occurrence will satisfy the requirement. 

### Code
```java
class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        int n = nums.length;
        if (n == 0) return false;
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        
        for (int i = 0; i < n; i++) {
            if (map.get(nums[i]) != null) {
                if (i - map.get(nums[i]) <= k) {
                    return true;
                } else {
                    map.put(nums[i], i);
                }
            } else {
                map.put(nums[i], i);
            }
        }
        
        return false;
    }
}
```

### Complexity analysis
* O(n) for time complexity, as we only traverse through the list once.
* O(n) for space complexity, as in the worst case all numbers are distince and we store every number.

## Solution 2
### Intuition
Since we are only looking for duplicates that occur within `k` numbers, we might want to use a sliding window. The reason is that
we don't care about the elements before the window. We use a set to keep track of all the numbers in the window. We slide the window
to the right by 1 element each time and remove the 1st element of the previous window from the set. We then add the new elemetn into the set.
If the element is already present in the set, return true.

### Code
```class Solution {
    public boolean containsNearbyDuplicate(int[] nums, int k) {
        int n = nums.length;
        if (n == 0) return false;
        Set<Integer> set = new HashSet<Integer>();
        
        for (int i = 0; i < n; i++) {
            if (i > k) {
                set.remove(nums[i - k - 1]);
            }
            if (!set.add(nums[i])) return true;
        }
        
        return false;
    }
}
```

### Complexity analysis
* O(n) for time complexity, as we need to iterate through the list once
* O(k) for space complexity, as we only keep track of the numbers in the window.
