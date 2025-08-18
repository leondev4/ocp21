---

markmap:
  initialExpandLevel: 2
---
# **Using Optional with Primitive Streams**
- You can calculate the average in one line.
  - ```js
    var stream = IntStream.rangeClosed(1,10);
    OptionalDouble optional = stream.average();
    ```
    - The return type is not the `Optional`. Why not just use `Optional<Double>`? The 
    difference is that `OptionalDouble` is for a primitive and `Optional<Double>` 
    is for the `Double` wrapper class. Working with the primitive optional class 
    looks similar to working with the `Optional` class itself.
      ```js
      optional.ifPresent(System.out::println);                  // 5.5
      System.out.println(optional.getAsDouble());               // 5.5
      System.out.println(optional.orElseGet(() -> Double.NaN)); // 5.5
      ```
      - The only noticeable difference is that we called `getAsDouble()` rather than
      `get()`. This makes it clear that we are working with a primitive. Also,
      `orElseGet()` takes a `DoubleSupplier` instead of a Supplier.
- **TABLE 10.8** Optional types for primitives
  ||`OptionalDouble`|`OptionalInt`|`OptionalLong`|
  |-|-|-|-|
  |Getting as primitive|`getAsDouble()`|`getAsInt()`|`getAsLong()`|
  |`orElseGet()` parameter type|`DoubleSupplier`|`IntSupplier`|`LongSupplier`|
  |Return type of `max()` <br/> and `min()`|`OptionalDouble`|`OptionalInt`|`OptionalLong`|
  |Return type of `sum()`|`double`|`int`|`long`|
  |`Return type of average()`|`OptionalDouble`|`OptionalDouble`|`OptionalDouble`|
    - Summary
      Class 
      ```
      OptionalX
      ```
      - Getting as primitive
        - ```
          getAsX()
          ```
      - `orElseGet()` parameter type
        - ```
          XSupplier
          ```
      - Return type of `max()` <br/> and `min()`
        - ```
          OptionalX
          ```
      - Return type of `sum()`
        - ```
          x
          ```
      - Return type of `average()`
        - ```
          OptionalDouble
          ```


- Let’s try an example to make 
  sure that you understand this:
  - ```js
    LongStream longs = LongStream.of(5, 10);

    long sum = longs.sum();
    System.out.println(sum);    // 15

    longs = LongStream.of(5, 10);

    var ints = longs.mapToInt(x -> (int)x);

    OptionalDouble avg = ints.average();
    System.out.println(avg.getAsDouble());//7.5
    
    DoubleStream doubles = DoubleStream.generate(() -> Math.PI);
    OptionalDouble min = doubles.min(); // runs infinitely
    ```
    - To use `collect` method, the stream must be `Stream<T>`
# **Summarizing Statistics**
- We want a method that take an `IntStream` and return a range. The range 
is the minimum value subtracted from the maximum value. Both `min()` 
and `max()` are terminal operations, which means that they use up the 
stream when they are run. We can’t run two terminal operations against 
the same stream. Luckily, this is a common problem, and the primitive 
streams solve it for us with summary statistics.
  - ```js
    private static int range(IntStream ints) {
      IntSummaryStatistics stats =            
          ints.summaryStatistics();
      if (stats.getCount() == 0) throw new  
        RuntimeException();
      return stats.getMax()-­stats.getMin();
    }
    ```
- Summary statistics 
  include the following:
  - **`getCount():`**
    - Returns a `long` representing the number of values.
  - **`getAverage():`**
    - Returns a `double` representing the average. If the
    stream is empty, returns `0`.
  - **`getSum()`:**
    - Returns the sum as a `double` for `DoubleSummaryStream` and 
    `long` for `IntSummaryStream` and `LongSummaryStream`.
  - **`getMin():`**
    - Returns the smallest number (minimum) as a `double` , `int`, 
    or `long`, depending on the type of the stream. If the stream 
    is empty, returns the largest numeric value based on the type.
  - **`getMax():`**
    - Returns the largest number (maximum) as a `double`, `int`, 
    or `long` depending on the type of the stream. If the stream 
    is empty, returns the smallest numeric value based on the type.