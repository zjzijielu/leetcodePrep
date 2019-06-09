# 83. Remove Duplicates from Sorted List
problem [link](https://leetcode.com/problems/remove-duplicates-from-sorted-list/)
## Imperfect approach
Two pointers `<prev>` and `<cur>`. Keep track of last value visited. If `<cur.val> == last`, `<prev.next> = <cur.next>`

```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        int last = 0;
        // ListNode dummyHead = head;
        ListNode ptr = head;
        ListNode prev = ptr;
        if (head != null) {
            last = head.val;
            ptr = ptr.next;
        } else {
            return head;
        }

        while (ptr != null) {
            if (ptr.val == last) {
                prev.next = ptr.next;
            } else {
                last = ptr.val;
                prev = prev.next;
            }
            ptr = ptr.next;
        }
        return head;
    }
}
```

## Improvement
One pointer `<cur>` is enough. If `<cur.val == cur.next.val>`, redirect `<cur.next>`

### code
```java
class Solution {
    public ListNode deleteDuplicates(ListNode head) {
        ListNode cur = head;
        while (cur != null && cur.next != null) {
            if (cur.next.val == cur.val) {
                cur.next = cur.next.next;
            } else {
                 cur = cur.next;
            }
        }
        return head;
    }
}
```

## What to notice
* Can directly return input ListNode `<head>` instead of making a dummy copy node
