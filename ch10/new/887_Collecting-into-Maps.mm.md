---

markmap:
  initialExpandLevel: 1
---
# **Collecting into Maps**
- you use `Collectors.toMap`
  - `(fun, fun)`
    - The _first function_ tells the collector _how to create the key_.  
    The _second function_ tells the collector _how to create the value_.
  - `(fun, fun, binOpe_merge)`
    - If there is a collision in the key generation, 
    add a `BinaryOperator` to resolve it.
  - `(fun, fun ,binOpe_merge, Supplier)`
    - If you want to change `Map` returned to `TreeMap`, give a `Supplier`
- Group by name and its length characters
  - ```js
    Map<String, Integer> map = Stream.of("Peter", "Brenda", "Cindy").
      collect(Collectors.toMap(k -> k, v -> v.length()));

    System.out.println(map);        //{Brenda=6, Peter=5, Cindy=5}
    System.out.println(map.getClass().getName());//java.util.HashMap
    ```
- If you just reverse the previous example, 
  there is a collision (`Duplicate key 5`). 
  You must use `BinaryOperator`. 
  - ```js
    Map<Integer, String> map = Stream.of("Peter", "Brenda", "Cindy").
      collect(Collectors.toMap(k -> k.length(), v -> v, (s1,s2)->s1+","+s2));

    System.out.println(map);        //{5=Peter,Cindy, 6=Brenda}
    System.out.println(map.getClass().getName());//java.util.HashMap
    ```
- The previous example but instead of 
  returning `Map` it returns `TreeMap`
  - ```js
    //TreeMap<Integer, String> map = Stream.of("Peter", "Brenda", "Cindy").collect( 
    Map<Integer, String> map = Stream.of("Peter", "Brenda", "Cindy").collect(
      Collectors.toMap(k -> k.length(), v -> v,  (s1, s2) -> s1 + "," + s2, TreeMap::new));

    System.out.println(map);        //{5=Peter,Cindy, 6=Brenda}
    System.out.println(map.getClass().getName());//java.util.TreeMap
    ```
    - We want to mandate that the code return a `TreeMap` instead