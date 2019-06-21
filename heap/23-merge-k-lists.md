# 23. Merge k Sorted Lists
problem [link](https://leetcode.com/problems/merge-k-sorted-lists/)

## Solution 
### Intuition
Use a min heap to store all the values. We pop the heap until the heap is empty and store the pop value in order.

There's no predefined heap structure in java, so we use priority queue instead.

### Code
```java
class Solution {
    public ListNode mergeKLists(ListNode[] lists) {
        int n = lists.length;
        if (n == 0) {
            return null;
        } 
        
        PriorityQueue<Integer> q = new PriorityQueue<Integer>();
        ListNode result = new ListNode(0);
        ListNode dummyStart = result;
        
        for (int i = 0; i < n; i++) {
            while (lists[i] != null) {
                q.add(lists[i].val);
                lists[i] = lists[i].next;
            }
        }
        
        while(q.size() != 0) {
            int val = q.remove();
            ListNode newNode = new ListNode(val);
            result.next = newNode;
            result = result.next;
        }
        return dummyStart.next;
    }
}
```

### Complexity Anlysis
* O(nlogn) for time complexity, since insert and extract both are O(logn) in heap.
* O(n) space complexity, as the heap stores all values.

## Comment
* Solve the problem by divide and conquer in the future
