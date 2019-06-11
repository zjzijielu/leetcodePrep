# 24. Swap Nodes in Pairs
problem [link](https://leetcode.com/problems/swap-nodes-in-pairs/)

## Solution
### Intuition
Use two pointers trailer and ptr. I use a helper function to swap ptr and ptr.next and make trailer.next = ptr.next. We iterate through the linked list, and make ptr and trailer skip two nodes upon each update.
### Code
```java
class Solution {
    public ListNode swapPairs(ListNode head) {
        if (head == null) {
            return head;
        }
        int count = 0;
        ListNode startNode = head;
        ListNode ptr = head;
        ListNode trailer = head;
        while (ptr != null && ptr.next != null) {
            if (count == 0) {
                startNode = swapHelper(null, ptr, ptr.next);
                trailer = startNode.next;
            } else {
                trailer = swapHelper(trailer, ptr, ptr.next);
                if (trailer.next != null) {
                    trailer = trailer.next.next;
                }
            }
            count++;
            if (trailer.next != null) {
                ptr = trailer.next;
            }
        }
        return startNode;
    }
    
    public ListNode swapHelper(ListNode head, ListNode node1, ListNode node2) {
        node1.next = node2.next;
        node2.next = node1;
        if (head != null) {
            head.next = node2;
        } else {
            return node2;
        }
        return head;
    }
}
```

### Complexity analysis
# O(N) for time complexity, every node will only be visited once.
# O(1) for space complexity, as we only need two extra pointers.

### Comment
The code is not symmetrical, as the trailer and ptr only skip one node after swaping the 1st pair. 

## A cleaner solution
### Intuition
At the beginning, add another dummy node `<dummy>`, and we return `<dummy.next>`.

### Code
```java
public ListNode swapPairs(ListNode head) {
    ListNode dummy = new ListNode(0);
    dummy.next = head;
    ListNode current = dummy;
    while (current.next != null && current.next.next != null) {
        ListNode first = current.next;
        ListNode second = current.next.next;
        first.next = second.next;
        current.next = second;
        current.next.next = first;
        current = current.next.next;
    }
    return dummy.next;
}
```
