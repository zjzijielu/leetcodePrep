# 134. Gas Station
problem [link](https://leetcode.com/problems/gas-station/)

## Solution 1
### Intuition 
We start from the beginning and keep track of the sum of gas subtracted by cost at each station. Once we see the sum 
turns negative, we move the start pointer to the next index of `curr`.

The reason is that if from point A to point B there is no valid solution, there's not going to be a solution given the starting
point is anywhere between A and B. So let `start = B + 1`

Here's a simple proof of the logic:

If `B-1 > A`, then that means `gas[A] - cost[A] > 0`. Otherwise, A can't even be a starting point. Knowing that, we know that 
sum of `gas[A:B] - cost[A:B] > gas[A+1:B] - cost[A+1:B]`. So moving the starting point to anywhere before B will be meaningless,
as the sum will never be as big.

### Code
```java
class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;
        int sum = 0;
        int total = 0;
        int count = 0;
        
        for (int i = 0; i < n; i++) {
            int j = i;
            while (count < n) {
                sum = sum + gas[j] - cost[j];
                if (sum < 0) {
                    sum = 0;
                    count = 0;
                    break;
                }
                count++;
                j = (j + 1) % n;
            }
            
            if (count == n) {
                return i;
            }
            
        }
        
        if (sum < 0) {
            return -1;
        }
        return -1;
        
    }
}
```

### Solution 2
Problem with my solution is that I traverse all of the list to check if starting from a certain point is valid. 
This makes the algorithm O(n^2). To avoid that, we can first compare if the sum of gas is larger than sum of cost.
If not, there can't be a solution. If yes, since it is gauranteed that there is a unique solution, then we can safely traverse
the list only once.
```java
public int canCompleteCircuit(int[] gas, int[] cost) {
    int tank = 0;
    for(int i = 0; i < gas.length; i++)
        tank += gas[i] - cost[i];
    if(tank < 0)
        return - 1;
        
    int start = 0;
    int accumulate = 0;
    for(int i = 0; i < gas.length; i++){
        int curGain = gas[i] - cost[i];
        if(accumulate + curGain < 0){
            start = i + 1;
            accumulate = 0;
        }
        else accumulate += curGain;
    }
    
    return start;
}
```

## Solution 3
### Intuition
Solution 3 is actually the smartest. We set two pointers `start = n - 1` and `end = 0`. We check if the current sum is larger than 0.
If so, we move `end` to the next index. If not, we move `start` backward.

### Code
```java
public int canCompleteCircuit(int[] gas, int[] cost) {
      int n = gas.length;
      int start = n - 1;
      int end = 0;
      int sum = gas[start] - cost[start];

      while (start > end) {
          if (sum < 0) {
              start--;
              sum += gas[start] - cost[start];
          } else {
              sum += gas[end] - cost[end];
              end++;
          }
      }

      return sum >= 0 ? start : -1;
  }
  ```
  
  ### Complexity analysis
  * O(n) for time complexity, as we only need to traverse the list once.
  * O(1) for space complexity.
