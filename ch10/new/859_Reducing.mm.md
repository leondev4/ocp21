---

markmap:
  initialExpandLevel: 1
---

# **Reducing**
- Combines a stream into a single object. It is a reduction,
 which means it processes all elements.
- ```java
  public T reduce(T identity, BinaryOperator<T> accumulator)
  ```
  - The _identity_ is the initial value of the reduction, in this case an empty `String`. The 
  _accumulator_ combines the current result with the current value in the stream.
  - ```js
    var list = List.of("A", "le", "x");
    var r = list.stream().reduce("", String::concat);
                      // .reduce("", (acc,str)->acc.concat(str));
    System.out.println(r);//Alex
    ```
  - ```js
    Stream<Integer> stream = Stream.of(1, 2, 3);
    System.out.println(stream.reduce(0, (acc, i) -> acc+i)); //6
    ```
- ```java
  public Optional<T> reduce(BinaryOperator<T> accumulator)
  ```
  - When you don’t specify an identity, an `Optional` 
  is returned because there might not be any data. 
    - If the stream is empty, an empty `Optional` is returned.
    - If the stream has one element, it is returned.
    - If the stream has multiple elements, the `accumulator`
     is applied to combine them.
  - ```
    BinaryOperator<Integer> op = (a, b) -­> a * b;
    Stream<Integer> empty = Stream.empty();
    Stream<Integer> oneElement = Stream.of(3);
    Stream<Integer> threeElements = Stream.of(3, 4, 5);
    ```
    - ```js
      empty.reduce(op).ifPresent(System.out::println);         // no output
      oneElement.reduce(op).ifPresent(System.out::println);    // 3
      threeElements.reduce(op).ifPresent(System.out::println); // 60
      ```
- ```js
  public <U> U reduce(U identity,
    BiFunction<U,? super T,U> accumulator,
    BinaryOperator<U> combiner)
  ```
  - It is used when we are dealing with different types. It
   allows Java to create intermediate reductions and 
   then combine them at the end
    - The first parameter is the value for the _initializer_.
    - Second parameter is the _accumulator_. This one handles mixed data types
    - The third parameter is called the _combiner_, which combines any intermediate 
    totals of the same type.
  - ```js
    Stream<String> stream = Stream.of("a", "le", "x");
    var r = stream.reduce(0, ( acc,s) -> acc + s.length(), 
      (x, y) -> x + y);
    System.out.println(r) //4
    ```
  - It is useful when working with parallel streams