---

markmap:
  initialExpandLevel: 1
---
# **Calculating with Math APIs**
- **Finding the Minimum and Maximum**
The `Math.min()` and `Math.max()` methods compare two values
and return one of them.
The method signatures for `Math.min()` are as follows:
  ```java
  public static double min(double a, double b)
  public static float min(float a, float b)
  public static int min(int a, int b)
  public static long min(long a, long b)
  ```

  - There are four overloaded methods, so you always have an
  API available with the same type. Each method returns
  whichever of `a` or `b` is smaller. The `max()` method works the
  same way, except it returns the larger value.
  The following shows how to use these methods:
    ```java
    int first = Math.max(3, 7);   // 7
    int second = Math.min(7, -9); // -9
    ```
    - The first line returns `7` because it is larger. The second line
    returns `-9` because it is smaller.
        - **Rounding Numbers**
        The `Math.round()` method gets rid of the decimal portion of
        the value, choosing the next higher number if appropriate.
        If the fractional part is `.5` or higher, we round up.
        The method signatures for `Math.round()` are as follows:
          ```java
          public static long round(double num)
          public static int round(float num)
          ```
          - There are two overloaded methods to ensure that there is
          enough room to store a rounded `double` if needed. The
          following shows how to use this method:
            ```java
            long low = Math.round(123.45); // 123
            long high = Math.round(123.50); // 124
            int fromFloat = Math.round(123.45f); // 123
            ```
            - The first line returns `123` because `.45` is smaller than a half.
            The second line returns `124` because the fractional part is
            just barely a half. The final line shows that an explicit `float`
            triggers the method signature that returns an `int`.
- **Determining the Ceiling and Floor**
The `Math.ceil()` method takes a `double` value. If it is a 
whole number, it returns the same value. If it has 
any fractional value, it rounds up to the next whole number.
 By contrast, the `Math.floor()` method discards any values 
 after  the decimal. The method signatures are as follows:
  ```java
  public static double ceil(double num)
  public static double floor(double num)
  ```
  - The following shows how to use these methods:
    ```java
    double c = Math.ceil(3.14); // 4.0
    double f = Math.floor(3.14); // 3.0
           c = Math.ceil(3); // 3.0
           f = Math.floor(3); // 3.0
    ```
    The first line returns `4.0` because four is the integer, just
    larger. The second line returns `3.0` because it is the integer,
    just smaller.
    - **Calculating Exponents**
    The `Math.pow()` method handles exponents. 
    The method signature is as follows:
      ```java
      public static double pow(double num, double exp)
      ```
      The following shows how to use this method:
      ```java
      double squared = Math.pow(5, 2); // 25.0
      ```
      Notice that the result is `25.0` rather than `25` since it is a
      `double`. 
      - **Generating Random Numbers**
      The `Math.random()` method returns a value greater than or
      equal to `0.0` and less than `1.0` (`0.0<=x<1.0`). The method 
      signature is as follows:
        ```java
        public static double random()
        ```

        - The following shows how to use this method:
          ```java
          double num = Math.random();
          ```
          Since it is a random number, we can’t know the result in
          advance. However, we can rule out certain numbers. For
          example, it can’t be negative because that’s less than `0.0`. It
          can’t be `1.0` because that’s not less than `1`.
- **Using `BigInteger` and `BigDecimal`**
Primitive types are not always precise. Especially
when dealing with money or large numbers. Java has
built in classes called `BigInteger` and `BigDecimal`. 
Like `String`, these classes are immutable so you 
chain methods to perform multiple operations.
    - While there are constructors, it is recommended to use the
  `valueOf()` method where possible. Note that you can pass a
  `long` to either type, but a `double` only to `BigDecimal`.
      ```java
      var bigInt = BigInteger.valueOf(5_000L);
      var bigDecimal = BigDecimal.valueOf(5_000L);
      bigDecimal = BigDecimal.valueOf(5_000.00);
      ```
      Both classes provide constants for the most common values
      like `BigInteger.ZERO` and `BigDecimal.ONE`.

      - There are methods to perform math operations with these
      types, such as:
        ```java
        var bigInt = BigInteger.valueOf(199)
            .add(BigInteger.valueOf(1))
            .divide(BigInteger.TEN)
            .max(BigInteger.valueOf(6));
        System.out.println(bigInt); // 20
        ```
        This example starts by adding `199` and `1` which gives `200`.
        It then divides by `10` resulting in `20`. Finally, `max()` sees that
        `20` is larger than `6` and we have the result.
      