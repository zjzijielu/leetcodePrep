# 142. Linked List Cycle II
problem [link](https://leetcode.com/problems/linked-list-cycle-ii/)

## O(1) memory Solution
### Intuition
The problem is very similar to linked list cycle. We use a slow pointer and a fast pointer to see if they can meet each other.
The tricky part is what happens afterwards. Suppose the cycle starts at the A-th node, while the two pointers meet at A+B-th node as follows
```java
/*
*      A   A+B
*      |    |   
* 3 -> 2 -> 0 -> 1 
*      |---------|
*/     
```
By the time `slow` and `fast` meet, `fast` have already traveled `2A+2B`, and `A+B+kN = 2A+2B`, and therefore `A+B = kN`, where `N` is the length of the cycle.
Which also means that for `fast` to go back to the start of the cycle, it needs to travel `A` more nodes.

That means if we place either pointer to the beginning and move each pointer one step at a time. By the time they meet each other, that node
will be where the cycle starts

### Code
```java
public class Solution {
    public ListNode detectCycle(ListNode head) {
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        
        ListNode ptr1 = dummy;
        ListNode ptr2 = dummy;
        
        while(ptr2.next != null && ptr2.next.next != null) {
            ptr1 = ptr1.next;
            ptr2 = ptr2.next.next;
            
            if (ptr1 == ptr2) {
                ListNode ptr3 = dummy;
                while (ptr3 != ptr1) {
                    ptr3 = ptr3.next;
                    ptr1 = ptr1.next;
                }
                
                return ptr3;
            }
               
        }
        
        return null;
    }
}
```

### Complexiy analysis
* TODO: TIME COMPLEXITY
* O(1) for space complexity
