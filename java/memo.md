# Some useful code for defining data structures
This is a memo to myself since I'm practicing leetCode in Java. 

## List
### Define an empty list
```java
List<Integer> emptyList = new ArrayList<Integer>();
```

### Define an empty list of lists
```java
List<List<Integer>> result = new ArrayList<List<Integer>>();
List<Integer> emptyList = new ArrayList<Integer>();
result.add(emtpyList);
```
