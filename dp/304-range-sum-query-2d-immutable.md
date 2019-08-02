# 304. Range Sum Query 2D - Immutable
problem [link](https://leetcode.com/problems/range-sum-query-2d-immutable/)

## Optimal Solution
### Intuition
Similar to how all subarray sums are computed, the submatrix sum is computed in a similar manner. For details, refer to [here](https://leetcode.com/problems/range-sum-query-2d-immutable/solution/)
for the graphs. What's worth mentioning here is how the sum of all elements in submatrix `[0,0]` to `[n,m]` is computed.
```java
cache = new int[rows + 1][cols + 1];
for (int i = 0; i < rows; i++) {
    for (int j = 0; j < cols; j++) {
        cache[i + 1][j + 1] = cache[i][j + 1] + cache[i + 1][j] + matrix[i][j] - cache[i][j]; // cache[i][j] is added twice
    }
}
```

### Code
```java
class NumMatrix {
    int[][] cache;
    public NumMatrix(int[][] matrix) {
        int rows = matrix.length;
        if (rows == 0) return;
        int cols = matrix[0].length;
        if (cols == 0) return;
        
        cache = new int[rows + 1][cols + 1];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < cols; j++) {
                cache[i + 1][j + 1] = cache[i][j + 1] + cache[i + 1][j] + matrix[i][j] - cache[i][j];
            }
        }
    }
    
    public int sumRegion(int row1, int col1, int row2, int col2) {
        int sum = 0;
        sum = cache[row2 + 1][col2 + 1];
        sum -= cache[row1][col1];
        sum -= (cache[row1][col2 + 1] - cache[row1][col1]);
        sum -= (cache[row2 + 1][col1] - cache[row1][col1]);
        return sum;
    }
}
```

### Complexity analysis
* time: O(1) for sumRegion. O(mn) at initialization
* space: O(mn)
