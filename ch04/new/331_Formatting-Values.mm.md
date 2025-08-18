---

markmap:
  initialExpandLevel: 1
---
# **Formatting Values** 
- There are methods to format `String` values using formatting
flags. Two of the methods take the format string as a
parameter, and the other uses an instance for that value.
One method takes a `Locale`
    - The method parameters are used to construct a formatted
    `String` in a single method call, rather than via a lot of format
    and concatenation operations. They return a reference to
    the instance they are called on so that operations can be
    chained together. The method signatures are as follows:

        - 
            ```java
            public static String format(String format, Object… args)
            public static String format(Locale loc, String format, Object… args)
            public String formatted(Object… args)
            ```
            - The following code shows how to use these methods:
                ```java
                var name = "Kate";
                var orderId = 5;
                // All print: Hello Kate, order 5 is ready
                System.out.println("Hello "+name+", order "+orderId+" is ready");
                System.out.println(String.format("Hello %s, order %d is ready",name, orderId));
                System.out.println("Hello %s, order %d is ready".formatted(name, orderId));
                ```
                - In the `format()` and `formatted()` operations, the parameters
                are inserted and formatted via symbols in the order that
                they are provided in the vararg. 
                    - **TABLE 4.2** Common formatting symbols
                    <br>
                        |&nbsp;Symbol&nbsp;|&nbsp;Description|
                        |:-:|-|
                        |`%s`|Applies to any type, commonly `String` values|
                        |`%d`|Applies to integer values like `int` and `long`|
                        |`%f`|Applies to floating-point values like `float` and `double`|
                        |`%n`|Inserts a line break using the system-dependent line separator|
- The following example uses all four symbols from Table 4.2:
    ```java
    var name = "James";
    var score = 90.25;
    var total = 100;
    System.out.println("%s:%n   Score: %f out of %d" 
        .formatted(name, score, total));
    ```
    - This prints the following:
        ```
        James:
            Score: 90.250000 out of 100
        ```
        Mixing data types may cause exceptions at runtime. For
        example, the following throws an exception because a
        floating-point number is used when an integer value is
        expected:
        ```
        var str = "Food: %d tons".formatted(2.0); //
        IllegalFormatConversionException
        ```
        - **Using `format()` with Flags**
            - Besides supporting symbols, Java also supports optional
            flags between the `%` and the symbol character. In the
            previous example, the floating-point number was printed
            as `90.250000`. By default, `%f` displays exactly six digits past
            the decimal. If you want to display only one digit after
            the decimal, you can use `%.1f` instead of `%f`. The `format()`
            method relies on rounding rather than truncating when
            shortening numbers. For example, `90.250000` will be
            displayed as `90.3` (not `90.2`) when passed to `format()` with
            `%.1f`.
                - The `format()` method also supports two additional
                features. You can specify the total length of output by
                using a number before the decimal symbol. By default,
                the method will fill the empty space with blank spaces.
                You can also fill the empty space with zeros by placing a
                single zero before the decimal symbol.
                    - The following
                    examples use brackets, `[]`, to show the start/end of the
                    formatted value:
                        ```java
                        var pi = 3.14159265359;
                        System.out.format("[%f]",pi);     // [3.141593]
                        System.out.format("[%12.8f]",pi); // [  3.14159265]
                        System.out.format("[%012f]",pi);  // [00003.141593]
                        System.out.format("[%12.2f]",pi); // [       3.14]
                        System.out.format("[%.3f]",pi);   // [3.142]
                        ```