---

markmap:
  initialExpandLevel: 2
---

# **Working with Primitive Streams**
- Have many of the same intermediate and terminal methods as a `Stream` but includes 
specialized methods for working with numeric data, like `sum()` and `average()`.
  - Types of primitive streams
    - ```
      IntStream
      ```
      - ```
        int, short, byte, char
        ```
    - ```
      LongStream
      ```
      - ```
        long
        ```
    - ```
      DoubleStream
      ```
      - ```java
        double, float
        ```

# **Common primitive stream methods**
- Watch carefully
  - `X` (uppercase) is `Int`, `Long`, `Double`
    - `x` (lowercase) is `int`, `long`, `double`
- Arithmetic mean of elements
  - ```
    OptionalDouble average()
    ```
    - ```
      XStream
      ```
- Stream<T> where T is wrapper class
associated with primitive value

  - ```java
    Stream<T> boxed()
    ```
    - ```
      XStream
      ```
- Maximum element of stream
  - ```
    OptionalX max()
    ```
    - ```
      XStream
      ```
- Minimum element of stream
  - ```
    OptionalX min()
    ```
    - ```
      XStream
      ```
- Returns primitive stream from `a`
(inclusive) to `b` (exclusive)
  - ```java
    XStream range(x a, x b)
    ```
    - ```
      XStream
      ```
      - Only `int` and `long`
- Returns primitive stream from `a`
(inclusive) to `b` (inclusive)
  - ```
    XStream rangeClosed(x a, x b)
    ```
    - ```
      XStream
      ```
      - Only `int` and `long`
- Returns sum of elements in
stream
  - ```
    x sum()
    ```
    - ```
      XStream
      ```
- Returns object containing
numerous stream statistics such
as average, min, max, etc.
  - ```
    XSummaryStatistics
      summaryStatistics()
    ```
    - ```
      XStream
      ```