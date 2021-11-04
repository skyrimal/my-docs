# 1. 数组转化为List：

```java
String[] strArray= new String[]{"Tom", "Bob", "Jane"};
List strList= Arrays.asList(strArray);
```

#  

# 2. 数组转Set

```java
String[] strArray= new String[]{"Tom", "Bob", "Jane"};
Set<String> staffsSet = new HashSet<>(Arrays.asList(staffs));
staffsSet.add("Mary"); // ok
staffsSet.remove("Tom"); // ok
```

#  

# 3. List转Set

```java
String[] staffs = new String[]{"Tom", "Bob", "Jane"};
List staffsList = Arrays.asList(staffs);
Set result = new HashSet(staffsList);
```

#  

# 4. set转List

```java
String[] staffs = new String[]{"Tom", "Bob", "Jane"};
Set<String> staffsSet = new HashSet<>(Arrays.asList(staffs));
List<String> result = new ArrayList<>(staffsSet);
```