# 148. Sort List
problem [link](https://leetcode.com/problems/sort-list/)

## Recursive solution
### Intuition
An intuitive way to solve the problem is to use divide and conquer. We split the lists into half and sort each half and then merge the two sublists.

### Code
```java
class Solution {
    public ListNode sortList(ListNode head) {
        if (head == null || head.next == null) {
            return head;
        }
        
        // split the list into to 2 parts
        ListNode prev = null, slow = head, fast = head;
        while (fast != null && fast.next != null) {
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        prev.next = null;
        
        // sort two lists
        ListNode l1 = sortList(head);
        ListNode l2 = sortList(slow);
        // merge them together
        l1 = mergeTwoLists(l1, l2);
        
        return l1;
           
    }
    
    public ListNode mergeTwoLists(ListNode l1, ListNode l2){
		if(l1 == null) return l2;
		if(l2 == null) return l1;
		if(l1.val < l2.val){
			l1.next = mergeTwoLists(l1.next, l2);
			return l1;
		} else {
			l2.next = mergeTwoLists(l1, l2.next);
			return l2;
		}
    }
}
```
### Complexity analysis
* O(nlogn) for time complexity, as we are doing merge sort.
* O(n) for space complexity, as recurssion stores a collection of backward pointers

## Constant memory solution
### Intuition
We are still doing divide and conquer but in an iterative, bottom-up way. The process is as follows:
```java
/*
* [4, 2, 3, 1, 5, 6]
* -> [4], [2], [3], [1], [5]// split them into sublists of size 1
* -> 2, 4, 1, 3, 5 // merge the sorted sublists into one
* -> [2, 4], [1, 3], [5] // split them into sublists of size 2
* -> 1, 2, 3, 4, 5 // merge the sorted sublists into one
* -> [1, 2, 3, 4], [5] // split them into sublists of size 4
* -> 1, 2, 3, 4, 5 // merge the sorted sublists into one
```
### Code
```java
class Solution {
    public ListNode sortList(ListNode head) {
        int n = 0;
        ListNode dummy = new ListNode(0);
        dummy.next = head;
        while (head != null) {
            n++;
            head = head.next;
        }
        
        for (int step = 1; step < n; step <<= 1) {
            ListNode prev = dummy;
            ListNode cur = dummy.next;
            while (cur != null) {
                ListNode left = cur;
                ListNode right = split(left, step);
                cur = split(right, step);
                prev = merge(left, right, prev);
            }
        }
        
        return dummy.next;
    }
    
    public ListNode split(ListNode start, int step) {
        if (start == null) {
            return null;
        }
        for (int i = 1; start.next != null && i < step; i++) {
            start = start.next;
        }
        
        ListNode right = start.next;
        start.next = null;
        
        return right;
    }
    
    public ListNode merge(ListNode l1, ListNode l2, ListNode tail) {
        while (l1 != null && l2 != null) {
            if (l1.val < l2.val) {
                tail.next = l1;
                l1 = l1.next;
            } else {
                tail.next = l2;
                l2 = l2.next;
            }
            tail = tail.next;
        }
        
        if (l1 != null) {
            tail.next = l1;
        }
        if (l2 != null) {
            tail.next = l2;
        }
        while (tail.next != null) {
            tail = tail.next;
        }
        
        return tail;
    }
}
```

### Complexity analysis
* O(nlogn) for time complexity
* O(1) for space complexity
