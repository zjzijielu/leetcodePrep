# 31. Next Permutation
problem [link](https://leetcode.com/problems/next-permutation/)

## Comment
I still managed to figure out the problem after coming up with examples and find the pattern which leads to the solution. 
I'm not so sure how the solution to this problem can be generalized to other problems.

## Solution

### Intuition
First, let's look at some examples:
* 3,1,4,7 -> 3,1,7,4
* 1,2,3 -> 1,3,2
The pair of number that we replace is the last increasing pair of digits. 

If there is no increasing pair of digits found at all, that must mean the numbers are decreasing, i.e. 3,2,1.
In such case, we reverse the whole int array.

Let's look at some other examples:
* 3,1,7,4 -> 3,4,1,7
* 1,3,2 -> 2,1,3
It's not enough to find the last increasing pair of digits. 
In the above scenarios, we can see that after locating the pair, the part following the increasing pair must be decreasing.
In order to get the next permutation, we have to reverse the decreasing digits.

Let's look at one more example:
* 8,3,7,4,2 -> 8,4,2,4,7
We can see that in the decreasing part, there are digits that are smaller than the 1st number of the increasing pair (i.e. 2 < 3).
Since we are looking for a bigger number, we swap the last digit in the decreasing pattern that is larger than our 1st number in the pair. In our case

```
8,3,7,4,2 -> 8,4,7,3,2
```

We can see that after swapping, the decreasing part is still decreasing. All we need to do now is to reverse the resulting decreasing part.

```
8,4,7,3,2 -> 8,4,2,3,7
```

### Code
```java
class Solution {
    public void nextPermutation(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return;
        }
        int prev = n - 2;
        int curr = n - 1;
        // locate the last increasing pattern
        while (prev >= 0 && nums[prev] >= nums[curr]) {
            prev--;
            curr--;
        }
        if (prev == -1) {
            for (int i = 0; i < n / 2; i++) {
                swap(nums, i, n - i - 1);
            }
        } else {
             // locate the last digit in the decreasing pattern that is larger than previous digit found
            int last = curr + 1;
            int mark = curr;
            while (last <= n - 1 && nums[curr] >= nums[last]) {
                if (nums[last] > nums[prev]) {
                    mark = last;
                }
                curr++;
                last++;
            }
            // swap the digits
            swap(nums, prev, mark);
            
            prev++;
            // reverse the decreasing pattern
            while (curr > prev) {
                swap(nums, curr, prev);
                prev++;
                curr--;
            }           
        }        
    }
    
    public void swap(int[] nums, int idx1, int idx2) {
        int buff = nums[idx1];
        nums[idx1] = nums[idx2];
        nums[idx2] = buff;
    }
}
```

### Better code
My code has some unnecessary pointers. I also have two parts of code that reverse the int array, which can be summarized into one function reverse.
```java
public class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while (i >= 0 && nums[i + 1] <= nums[i]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.length - 1;
            while (j >= 0 && nums[j] <= nums[i]) {
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i + 1);
    }

    private void reverse(int[] nums, int start) {
        int i = start, j = nums.length - 1;
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```
