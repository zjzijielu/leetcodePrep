# 88. Merge Sorted Array
problem [link](https://leetcode.com/problems/merge-sorted-array/)

## Solution
### Intuition
After seeing the hint, I realize merge sort can also be done in the other way around. We start from the end of the both arrays
and merge backwards. We use two pointers to keep track of the elements that we are looking at in both arrays, and place the bigger one
at the end of `nums1`.

### Code
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
      int len = nums1.length;
      int p1 = m - 1;
      int p2 = n - 1;

      for (int i = len - 1; i >= 0; i--) {
          if (p1 >= 0 && p2 >= 0) {
              if (nums1[p1] >= nums2[p2]) {
                  nums1[i] = nums1[p1];
                  p1--;
              } else {
                  nums1[i] = nums2[p2];
                  p2--;
              }
          } else if (p1 >= 0) {
              nums1[i] = nums1[p1];
              p1--;
          } else {
              nums1[i] = nums2[p2];
              p2--;
          }
      }
  }
```

### Complexity analysis
* O(n+m) for time complexity as we only go through both lists once
* O(n+m) for space complexity as we do in-place change

## Improved solution
The intuition is that if after merging, nums1 hasn't been traversed all the way to the beginning, there's no need to traverse any more
as leaving the elements the way they are already gaurantees that they are the smalles elements of both arrays.

### Code
```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
      int k = nums1.length - 1;
      int p1 = m - 1;
      int p2 = n - 1;

      while (p1 >= 0 && p2 >= 0) {
          if (nums1[p1] >= nums2[p2]) {
              nums1[k--] = nums1[p1--];
          } else {
              nums1[k--] = nums2[p2--];
          }
      }

      while (p2 >= 0) {
          nums1[k--] = nums2[p2--];
      }
  }
```
