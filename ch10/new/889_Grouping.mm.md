---

markmap:
  initialExpandLevel: 1
---
# **Grouping**
- You use `Collectors.groupingBy`
  - `(fun)`
  - `(fun,collector)`
  - `(fun,supplier,collector)`
- Note that the function that you call in `groupingBy()`
 cannot return `null`. It does not allow null keys
- Group by length:
  ```js
  Map<Integer, List<String>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.groupingBy(String::length));

  System.out.println(map);//{5=[Peter, Cindy], 6=[Brenda]}
  ```
  - Each value in the `Map` is a `List` 
- ```js
  Map<Integer, Set<String>> map = Stream.of("Peter", "Brenda", "Cindy")
    .collect(Collectors.groupingBy(String::length,Collectors.toSet()));

  System.out.println(map);//{5=[Peter, Cindy], 6=[Brenda]}
  ```
  - We don’t want a `List` as the value in the map and prefer a `Set` instead.
- ```js
  //Map<Integer, Set<String>> map = Stream.of("Peter", "Brenda", "Cindy")
  TreeMap<Integer, Set<String>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.groupingBy(String::length,TreeMap::new, Collectors..toSet()));

  System.out.println(map);//{5=[Peter, Cindy], 6=[Brenda]}
  ```
  - We can change the type of `Map` returned
- ```js
  TreeMap<Integer, List<String>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.groupingBy(String::length,TreeMap::new,Collectors.toList()));

  System.out.println(map);//{5=[Peter, Cindy], 6=[Brenda]}
  ```
  - We want to change the type of `Map` returned but leave the
  type of values as a `List`
- ```js
  Map<Integer, Long> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.groupingBy(String::length, Collectors.counting()));

  System.out.println(map);//{5=2, 6=1}
  ```
  - We can group by the length of the name to see how many of each length we have