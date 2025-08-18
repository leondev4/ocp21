---

markmap:
  initialExpandLevel: 1
---
# **Putting Together the Pipeline**
-  We want to get the first two names of our friends
 alphabetically that are four characters long
    - ```js
      var list = List.of("Toby", "Anna", "Leroy", "Alex");
      list.stream().filter(n -> n.length() == 4).sorted()
        .limit(2).forEach(System.out::println);
      ```
    - ```js
      var list = List.of("Toby", "Anna", "Leroy", "Alex");
      list.stream()
          .filter(n -> n.length() == 4)
          .sorted()
          .limit(2)
          .forEach(System.out::println);
      ```
      - We express what is going on. We filter `String` objects of length `4`. 
      Then we want them sorted. Then we want the first two. Then we 
      want to print them out.
- ```js
  Stream.generate(() -> "Toby")
    .filter(n -> n.length() == 4)
    .sorted() //It hangs here
    .limit(2)
    .forEach(System.out::println);
  ```
- ```js
  Stream.generate(() -> "Alex")
    .filter(n -> n.length() == 4)
    .limit(2)
    .sorted()
    .forEach(System.out::println);
  ```
  - This one prints `Alex` twice
- ```js
  Stream.generate(() -> "King Julien")
    .filter(n -> n.length() == 4) //It hangs here
    .limit(2)
    .sorted()
    .forEach(System.out::println);
    ```
- You can even chain two pipelines together:
  ```js
  30: long count = Stream.of("Alex Alakay", "Marty")
  31:  .filter(s -> s.length()> 5)
  32:  .collect(Collectors.toList())
  33:  .stream()
  34:  .count();
  35: System.out.println(count); // 1
  ```
  - Lines 30–32 are one pipeline, and lines 33 and 34 are another. For 
  the first pipeline, line 30 is the source, and line 32 is the terminal 
  operation. For the second pipeline, line 33 is the source, and line 
  34 is the terminal operation