---

markmap:
  initialExpandLevel: 1
---
# **Common Terminal Operations**
- count
  - ```
    public long count()
    ```
    - ```js
      Stream<String> s = Stream.of("a", "b", "c");
      System.out.println(s.count()); // 3
      ```
- min
  max
  - ```js
    public Optional<T> min(Comparator<? super T> comparator)
    ```
    - ```js
      Stream<String> s = Stream.of("monkey", "ape", "bonobo");
      Optional<String> min = s.min(Comparator.comparing(String::length));
                          // s.min((s1, s2) -­> s1.length()-­ s2.length());
      min.ifPresent(System.out::println); // ape
      ```
  - ```js
    public Optional<T> max(Comparator<? super T> comparator)
    ```
    - If the stream is empty, the comparator is never called. An empty 
    `Optional` is returned.
      ```js
      Optional<?> minEmpty = Stream.empty().max((s1, s2) -> 0);
      System.out.println(minEmpty.isPresent()); // false
      ```
- findAny
  findFirst
  - Return an element of the stream unless
  the stream is empty. If the stream is empty, 
  they return an empty `Optional`.
    - ```
      public Optional<T> findAny()
      ```
      - Can return any element of the stream.  Commonly returns the first element, 
      although this behavior is not guaranteed
      - ```js
        Stream<String> s = Stream.of("monkey", "gorilla", "bonobo");
        Stream<String> infinite = Stream.generate(() -­> "chimp");
        s.findAny().ifPresent(System.out::println); //monkey (usually)
        infinite.findAny().ifPresent(System.out::println); // chimp
        ```
    - ```
      public Optional<T> findFirst()
      ```
- Matching
  - ```js
    public boolean anyMatch(Predicate <? super T> predicate)
    ```

  - ```js
    public boolean allMatch(Predicate <? super T> predicate)
    ```
    - All satisfy the predicate
    - ```js
      var list = List.of("monkey", "2", "chimp");
      Stream<String> infinite = Stream.generate(() -­> "chimp");
      Predicate<String> pred = x -­> Character.isLetter(x.charAt(0));
                                                        //visits
      var r1 = list.stream().anyMatch(pred); // true    monkey
      var r2 = list.stream().allMatch(pred); // false   monkey,2
      var r3 = list.stream().noneMatch(pred); // false  monkey
      var r4 = infinite.anyMatch(pred);       // true   chimp
      ```
          - Any satisfy the predicate
      - Calling `anyMatch()` on the infinite stream is fine because we match right 
      away and the call terminates. However, consider what happens if you try
      calling `allMatch()`:
        ```js
        Stream<String> infinite = Stream.generate(() -> "chimp");
        Predicate<String> pred = x -> Character.isLetter(x.charAt(0));
        System.out.println(infinite.allMatch(pred)); // Never terminates
        ```
        Because `allMatch()` needs to check every element, it will run until we kill the
        program. 
  - ```js
    public boolean noneMatch(Predicate <? super T> predicate)
    ```
    - None satisfy the predicate
- Iterating
  -  Only terminal operation with a return type of `void`
  - ```js
    public void forEach(Consumer<? super T> action)
    ```
    - ```js
      var list = List.of("alex", "gloria", "marty");
      list.stream().forEach
        (System.out::print);//alexgloriamarty
        ```