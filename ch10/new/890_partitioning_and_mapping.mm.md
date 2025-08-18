---

markmap:
  initialExpandLevel: 2
---
# **Partitioning**
- You use `Collectors.partitioningBy`
  - `(predicate)`
  - `(predicate,collector)`
- Partitioning is a special case of grouping. With partitioning, there are only two possible
groups: `true` and `false`. Partitioning is like splitting a list into two parts.
- ```js
  Map<Boolean, List<String>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.partitioningBy(x -> x.length()<=5));

  System.out.println(map);//{false=[Brenda], true=[Peter, Cindy]}
  ```
  - Filter names with five or fewer character
- ```js
  Map<Boolean, List<String>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.partitioningBy(x -> x.length()<=7));

  System.out.println(map);//{false=[], true=[Peter, Brenda, Cindy]}
  ```
  - Filter names with seven or fewer character
- ```js
  Map<Boolean, Set<String>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.partitioningBy(x -> x.length()<=5,Collectors.toSet()));

  System.out.println(map);//{false=[Brenda], true=[Peter, Cindy]}
  ```
  - In the returned map change the value of `List` to `Set`.
# **mapping collector**
- we want to get the first letter of the first name alphabetically of each length.
- ```js
  Map<Integer, Optional<Character>> map = Stream.of("Peter", "Brenda", "Cindy").
    collect(Collectors.groupingBy(String::length, 
      Collectors.mapping(s -> s.charAt(0),Collectors.minBy((a, b) -> a - b))));

  System.out.println(map);//{5=Optional[C], 6=Optional[B]}
  ```
- `mapping()` takes two parameters: the function for the value and how to group it further
# **Teeing Collectors**
- You can use `teeing(Collector,Collector,BiFunction)` 
to return multiple values of your own.
  - First, define the return type. We use a record here:
    ```js
    record Separations(String spaceSeparated, String commaSeparated) {}
    ```
    - ```js
      var list = List.of("x", "y", "z");
      Separations result = list.stream()
        .collect(Collectors.teeing(
          Collectors.joining(" "),
          Collectors.joining(","),
          (s, c) -> new Separations(s, c)));
      System.out.println(result);
      ```
      When executed, the code prints the following:
      ```js
      Separations[spaceSeparated=x y z, commaSeparated=x,y,z]
      ```
      - There are three `Collectors` in this code. Two of them are for `joining()` and
      produce the values we want to return. The third is  `teeing()`, which combines
      the results into the single object we want to return.
- ```js
  List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

  // Calculate both sum and average using Collectors.teeing()
  String result = numbers.stream().collect(
    Collectors.teeing(
      Collectors.summingInt(Integer::intValue), // First collector: sums the integers
      Collectors.averagingDouble(Integer::intValue), // Second collector: calculates the average
      (sum, average) -> String.format("Sum: %d, Average: %.2f", sum, average) // Merger function
    ));

  System.out.println(result); // Sum: 55, Average: 5,50
  ```