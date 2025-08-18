---

markmap:
  initialExpandLevel: 2
---
# **creating a primitive stream**
- ```java
  DoubleStream empty = DoubleStream.empty();
  ```
  -  Empty stream
- ```java
  DoubleStream oneValue = DoubleStream.of(3.14);
  ```
  -  Single value
- ```java
  DoubleStream varargs = DoubleStream.of(1.0, 1.1, 1.2);
  ```
  - Using varargs 
- ```js
  var random = DoubleStream.generate(Math::random);
  var fractions = DoubleStream.iterate(.5, d -> d / 2);
  random.limit(3).forEach(System.out::println);
  fractions.limit(3).forEach(System.out::println);
  ```
  - Infinity stream,it prints:
    ```js
    0.28316890416066265
    0.17681797643995478
    0.5789314217149308
    0.5
    0.25
    0.125
    ```
- ```js
  var fractions = DoubleStream.iterate(
    .5,           //double seed
    d-> d > .125, //DoublePredicate
    d -> d / 2);  //DoubleUnaryOperator
  fractions.forEach(System.out::println);
  ```
  - It prints:
    ```js
    0.5
    0.25
    ```
- ```js
  IntStream range = IntStream.range(1, 6);
  range.forEach(System.out::print); // 12345
  ```
- ```js
  IntStream rangeClosed = IntStream.rangeClosed(1, 5);
  rangeClosed.forEach(System.out::print); // 12345
  ```
# **Mapping Streams**
- **TABLE 10.6** Mapping methods between types of streams
  |Source<br/>stream|To create<br/>Stream|To create <br/>DoubleStream|To create<br/>IntStream|To create<br/>LongStream|
  |-|-|-|-|-|
  |`Stream<T>`|`map()`|`mapToDouble()`|`mapToInt()`|`mapToLong()`|
  |`DoubleStream`|`mapToObj()`|`map()`|`mapToInt()`|`mapToLong()`|
  |`IntStream`|`mapToObj()`|`mapToDouble()`|`map()`|`mapToLong()`|
  |`LongStream`|`mapToObj()`|`mapToDouble()`|`mapToInt()`|`map()`|
  - Watch carefully
    - `X` (uppercase) is `Int`, `Long`, `Double`
      - `Y` (uppercase) is `Int`, `Long`, `Double`
  - Summary
    - Source stream 
      class
      - To create
       `Stream`
        - To create  
        `XStream`
    - `Stream<T>`   
      - `map()`
        - `mapToX()`
    - `YStream`   
      - `mapToObj()`       
        - `mapToX()`  
    - If `X` and `Y` are equals use `map()`. You can replace `mapToObj()` to `boxed()`.
- Obviously, they have to be compatible types for this to work. Java requires a
mapping function to be provided as a parameter, for example:
  ```js
  Stream<String> objStream = Stream.of("penguin", "fish");
  IntStream intStream = objStream.mapToInt(s -> s.length());
  ```
  This function takes an `Object`, which is a `String` in this case. The function
  returns an `int`. The function mappings are intuitive here. They take the source
  type and return the target type. In this example, the actual function type is
  `ToIntFunction`.
# **using flatMap**
- `XStream`
  - `flatMapToX`
    - `X` is `Int`, `Long`, `Double`
- We can use this approach on primitive streams as well. It works the same
way as on a regular Stream, except the method name is different. Here’s an
example:
  ```js
  var integerList = new ArrayList<Integer>();
  IntStream ints = integerList.stream()
    .flatMapToInt(x -> IntStream.of(x));
  DoubleStream doubles = integerList.stream()
    .flatMapToDouble(x -> DoubleStream.of(x));
  LongStream longs = integerList.stream()
    .flatMapToLong(x -> LongStream.of(x));
  ```
# **The mapping function<br/>names**
- **TABLE 10.7** Function parameters when mapping between types of streams
  |Source<br/>stream|To create<br/>Stream|To create<br/>DoubleStream|To create<br/>IntStream|To create<br/>LongStream|
  |-|-|-|-|-|
  |`Stream<T>`|`Function<T,R>`|`ToDoubleFunction<T>`|`ToIntFunction<T>`|`ToLongFunction<T>`|
  |`DoubleStream`|`DoubleFunction<R>`|`DoubleUnaryOperator`|`DoubleToIntFunction`|`DoubleToLongFunction`|
  |`IntStream`|`IntFunction<R>`|`IntToDoubleFunction`|`IntUnaryOperator`|`IntToLongFunction`|
  |`LongStream`|`LongFunction<R>`|`LongToDoubleFunction`|`LongToIntFunction`|`LongUnaryOperator`|
  - Summary
    - Watch carefully
      - `X`  is `Int`, `Long`, `Double`
        - `Y`  is `Int`, `Long`, `Double`
    - Source stream 
      class
      - To create 
      `Stream`
        - To create  
        `XStream`
    - `Stream<T>`   
      - `Function<T,R>`
        - `ToXFunction`
    - `YStream`       
      - `YFunction<R>`   
        - `YToXFunction`
    - If `X` and `Y` are equals use `XUnaryOperator`