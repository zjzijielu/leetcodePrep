# 189. Rotate Array
problem [link](https://leetcode.com/problems/rotate-array/)

## Imperfect solution
### Intuition
Initialze another array of equal size, first copy the last k elements, then copy the rest n-k elements.
### Code
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;
        int[] result = new int[n];
        for (int i = 0; i < n; i++) {
            result[i] = nums[i];
        }
        for (int i = 0; i < k; i++) {
            nums[i] = result[n - k + i];
        }
        for (int i = k; i < n; i++) {
            nums[i] = result[i - k];
        }
    }
}
```
### Problem
O(N) for both time and space complexity

## Better solution 1
### Intuition
Swap the elements along the chain of (i, (i+k)%n, (i+2k)%n, ...) The chain will reach back to i eventually (proof omitted). We also keep a counter to keep track of the number of the elements changed to the right position. We won't encounter the elements that are changed to the right place (proof omitted as well).
### Code
```java
class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;
        int count = 0;
        for (int start = 0; count < n; start++) {
            int current = start;
            int prev = nums[start];
            do {
                int next = (current + k) % n;
                int buff = nums[next];
                nums[next] = prev;
                prev = buff;
                count++;
                current = next;
            } while ( != current);
        }
    }
}
```
### Analysis
* O(N) for time complexity, only one pass through the list is enough.
* O(1) for space complexity. Only one buffer is enough.
## Comment
Good to remember the property mentioned in better solution 1 to swap elements in plance in an array.
