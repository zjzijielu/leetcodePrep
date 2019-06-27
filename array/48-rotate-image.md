# 48. Rotate Image
problem [link](https://leetcode.com/problems/rotate-image/)

## My solution
### Intuition 
Rotate the matrix layer by layer. In each layer, rotate the corresponding positions one by one. For example,
```java
/*
  1 2 3      7 2 1      7 4 1
  4 5 6  ->  4 5 6  ->  8 5 2
  7 8 9      9 8 3      9 6 3
*/
```

### Code
```java
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; i++) {
            int len = n - i * 2;
            for (int j = i; j < len + i - 1 ; j++) {
                helper(i, j, len, matrix);
            }
        }
    }
    
    public void helper(int i, int j, int len, int[][] matrix) {
        int buff = matrix[i][j];
        matrix[i][j] = matrix[2 * i + len - 1 - j][i];
        matrix[2 * i + len - 1 - j][i] = matrix[i + len - 1][2 * i + len - 1 - j];
        matrix[i + len - 1][2 * i + len - 1 - j] = matrix[j][i + len - 1];
        matrix[j][i + len - 1] = buff;
    }
}
```

### Complexity Analysis
* O(n^2) for time complexity, as we are traversing most oftenly all the values
* O(1) for space complexity, as we only need a buffer

## A more general solution
### Intuition
Clockwise rotate = reverse upside down + then swap along the y = -x symmetry

### Code
```
class Solution {
    public void rotate(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; i++) {
            for (int j = 0; j < n; j++) {
                swap(matrix, i, j, n - 1 - i, j);
            }
        }
        
        for (int i = 0; i < n; i++) {
            for (int j = i; j < n; j++) {
                swap(matrix, i, j, j, i);
            }
        }
    }
    
    public void swap(int[][] matrix, int i1, int j1, int i2, int j2) {
        int buffer = matrix[i1][j1];
        matrix[i1][j1] = matrix[i2][j2];
        matrix[i2][j2] = buffer;
    }
}
```

### Comment 
It's better to memorize the more general solution, as it took a while for me to figure out how to rotate layer by layer.
