---

markmap:
  initialExpandLevel: 2
---
# **Collecting Results**
- There are many predefined collectors, including those shown in **Table 10.10**. 
These collectors are available via `static` methods on the `Collectors` class. 
You can watch a [summary](https://1drv.ms/i/c/c83cfca51d5c2032/Eczd48qoHzdCta6En7pQ_wMBMkWgNYrlPbHbe-vM9vyplA?e=PN47BL)
  - **TABLE 10.10** Examples of grouping/partitioning collectors
    |Collector|Description|Return value when <br/>passed to `collect`|
    |-|-|-|
    |`averagingDouble( ToDoubleFunction f)`<br/>`averagingInt( ToIntFunction f)`<br/>`averagingLong( ToLongFunction f)`|Calculates average for three core primitive types|`Double`|
    |`counting()`|Counts number of elements|`Long`|
    |`filtering(Predicate p,Collector c)`|Applies filter before calling downstream collector|`R`|
    |`groupingBy(Function f)` <br/> `groupingBy(Function f, Collector dc)`<br/> `groupingBy(Function f, Supplier s, Collector dc)`|Creates map grouping by specified function with<br/>optional map type supplier and optional <br/>downstream collector of type `D`|`Map<K, List<T>>`<br/>`Map<K, List<D>>`<br/>`Map<K, List<D>>`|
    |`joining(CharSequence cs)`|Creates single `String` using `cs` as delimiter<br/>between elements if one is specified|`String`|
    |`maxBy(Comparator c)`<br/>`minBy(Comparator c)`|Finds largest/smallest elements|`Optional<T>`|
    |`mapping(Function f, Collector dc)`|Adds another level of collectors|`Collector`|
    |`partitioningBy(Predicate p)`<br/> `partitioningBy(Predicate p, Collector dc)`|Creates map grouping by specified predicate with<br/>optional further downstream collector|`Map<Boolean, List<T>>`|
    |`summarizingDouble( ToDoubleFunction f)`<br/>`summarizingInt( ToIntFunction f)`<br/>`summarizingLong(ToLongFunction f)`|Calculates average, min, max, etc.|`DoubleSummaryStatistics`<br/>`IntSummaryStatistics`<br/>`LongSummaryStatistics`|
    |`summingDouble(ToDoubleFunction f)`<br/>`summingInt( ToIntFunction f)`<br/>`summingLong(ToLongFunction f)`|Calculates sum for our three core primitive types|`Double`<br/>`Integer`<br/>`Long`|
    |`teeing(Collector c1, Collector c2, BiFunction f)`|Works with results of two collectors to create new<br/>type|`R`|
    |`toList()`<br/>`toSet()`|Creates arbitrary type of list or set|`List`<br/>`Set`|
    |`toCollection(Supplier s)`|Creates Collection of specified type|`Collection`|
    |`toMap(Function k, Function v)`<br/>`toMap(Function k, Function v, ` <br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`BinaryOperator m)`<br/> `toMap(Function k, Function v,`<br/> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`BinaryOperator m, Supplier s)`|Creates map using functions to map keys,values, <br/>optional merge function, and optional map type<br/>supplier|`Map`|
# **Using Basic Collectors**
- ```js
  var list = Arrays.asList("marty", "gloria", "alex");
  String result = list.stream().collect(Collectors.joining());
  System.out.println(result);             // martygloriaalex
  result = list.stream().collect(Collectors.joining(", "));
  System.out.println(result);             // marty, gloria, alex
  result = list.stream().collect(Collectors.joining(",", "[", "]"));
  System.out.println(result);             // [marty,gloria,alex]
  ```
- What is the average length of the three animal names?

  ```js
  var s = Stream.of("lions", "tigers", "bears");
  Double result = s.collect(Collectors.averagingInt(String::length));
  System.out.println(result); // 5.333333333333333
  ```
  - With primitive streams, the result of an average was always a `double` (in 
  `summaryStatistics()`, the method `average()` returns `OptionalDouble`), 
  regardless of what type is being averaged. For _collectors_, it is a `Double` 
  since those need an `Object`.
- You can work to using a `Stream` and then convert to a
 `Collection` at the end. For example:
  ```js
  var s = Stream.of("lions", "tigers", "bears");
  TreeSet<String> result = s
    .filter(s -> s.startsWith("t"))
    .collect(Collectors.toCollection(TreeSet::new));
  System.out.println(result); // [tigers]
  ```
- Using `toList()`
  - Both of these do almost the same thing:
    ```js
    Stream<String> ohMy1 = Stream.of("lions", "tigers", "bears");
    List<String> mutableList = ohMy1.collect(Collectors.toList());

    Stream<String> ohMy2 = Stream.of("lions", "tigers", "bears");
    List<String> immutableList = ohMy2.toList();
    ```
    - Both return a `List<String>`. The `Collectors.toList()` gives you a mutable 
    list that you can edit later. The shorter `toList()` does not allow changes.
      ```
      mutableList.add("zebras");   // No issues
      immutableList.add("zebras"); // UnsupportedOperationException
        ```