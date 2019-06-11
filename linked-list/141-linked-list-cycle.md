# 141.Linked List Cycle
problem [link](https://leetcode.com/problems/linked-list-cycle/)

## Solution 1
### Intuition
Use a hashtable to record all referenced addresses. If one address reoccurs, then there is a cycle.

### Code
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        Hashtable ht = new Hashtable();
        while (head != null && head.next != null) {
            if (ht.containsKey(head.next)) {
                return true;
            } else {
                ht.put(head.next, 1);
            }
            head = head.next;
        }
        return false;
    }
}
```
### Complexity analysis
* time: O(N), we only have to go through the list once
* space: O(N), one (key, value) pair for distinct references
Is it possible to use O(1) memory?

## Better Solution
### Intuition
We set up two pointers. Ptr1 moves 1 step at a time, ptr2 moves 2 steps at a time. Since it's a cycle, ptr2 will always be able to catch up with ptr1.

### Code
```java
public class Solution {
    public boolean hasCycle(ListNode head) {
        if (head == null) {
            return false;
        }
        ListNode slow = head;
        ListNode fast = head;
        do {
            if (slow != null) {
                slow = slow.next;
            } else {
                return false;
            }
            if (fast != null && fast.next != null) {
                fast = fast.next.next;
            } else {
                return false;
            }
        } while (slow != fast);
        return true;
    }
}
```
### Complexity analysis
* If there is a cycle, ptr 2 will first go through the first cycle, which is N/2 steps. Suppose by the time he arrives at the beginning of the cycle, and there is K steps between ptr1 and ptr2, then it will take K < N steps for ptr2 to catch up with ptr1. So time complexity is O(N/2 + K) = O(N).
* We only require two extra pointers, so memory complexity is O(1).

## Comment
* I thought there might be a way to use bit manipulation to detect the first replica in a list. So far I haven't come up with any plausible solution.
