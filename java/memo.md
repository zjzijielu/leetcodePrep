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

## HashMap/Hashtable

### Define a hash map
```java
Map<String, String> phone = new HashMap<String, String>() {{
    put("2", "abc");
    put("3", "def");
    put("4", "ghi");
    put("5", "jkl");
    put("6", "mno");
    put("7", "pqrs");
    put("8", "tuv");
    put("9", "wxyz");

}};
```
