---

markmap:
  initialExpandLevel: 1
---
# **Converting from String Using  <br/> `valueOf()` and `parseXxx()` Methods**
- ```
  int primitive = Integer.parseInt("123");
  Integer wrapper = Integer.valueOf("123");
  ```
  - The first line converts a `String` to an `int` primitive. The
  second converts a `String` to an `Integer` wrapper class.
  - On the numeric wrapper classes, `valueOf()` and `parseXxx()`
  throws a `NumberFormatException` on invalid input. 
    - ```java
      var var1 = Integer.valueOf("five");           //throws exception
      var val1 = Byte.parseByte("ABA");              //throws exception
      double double1 = Double.parseDouble("12.3D");  //compiles
      Float float1 = Float.valueOf("12.3F");         //compiles
      ```
- On `Boolean`, the method returns `Boolean.TRUE` for any value that 
matches `"true"` ignoring case, and `Boolean.FALSE` otherwise.
  - ```java
    System.out.println(Boolean.valueOf("true")); // true
    System.out.println(Boolean.valueOf("TrUe")); // true
    ```
  - ```java
    System.out.println(Boolean.valueOf("false")); // false
    System.out.println(Boolean.valueOf("FALSE")); // false
    System.out.println(Boolean.valueOf("kangaroo")); // false
    System.out.println(Boolean.valueOf(null)); // false
    ```
- The numeric integral classes (`Byte`, `Short`, `Integer`, and
`Long`) include an overloaded `valueOf(String str, int base)`
and `parseXxx(String s, int base)` methods that take a base
value. For hexadecimal, ignores case.
  - ```java
    var int1 = Integer.parseInt("AB_cA", 16);      //throws exception
    var int2 = Integer.parseInt("0xABcA", 16);     //throws exception
    var int3 = Integer.parseInt("ABcA",16);        //compiles: 43978
    var val1 = Byte.parseByte("ABA", 16);          //throws exception
    Short s = Short.valueOf("1010",2);             //compiles: 10
    System.out.println(Integer.valueOf("5", 16)); // 5
    System.out.println(Integer.valueOf("a", 16)); // 10
    System.out.println(Integer.valueOf("F", 16)); // 15
    System.out.println(Integer.valueOf("G", 16)); // throws exception
    ```
- All of the numeric classes extend the `Number` class, which means 
they all come with some useful helper methods: `byteValue()`,
`shortValue()`, `intValue()`, `longValue()`, `floatValue()`, and `doubleValue()`. 
The `Boolean` and `Character` wrapper classes include `booleanValue()`
 and `charValue()`.
  - These methods return the primitive value
of a wrapper instance
    - ```java
      Double apple = Double.valueOf("200.99");
      System.out.println(apple.byteValue()); // -56
      System.out.println(apple.intValue()); // 200
      System.out.println(apple.doubleValue()); // 200.99
      ```
    - In the first example, there is no 200 in `byte`, so it wraps around
    to `-56`. In the second example, the value is truncated, which means 
    all of the numbers after the decimal are dropped.
  - `Byte`, `Short`, `Integer`, `Long`, `Float` and `Double`
  wrapper classes contain this methods
- Some of the wrapper classes contain additional helper methods for
working with numbers.
  - ```java
    max(int num1, int num2) //which returns the largest of the two numbers
    min(int num1, int num2) //which returns the smallest of the two numbers
    sum(int num1, int num2) //which adds the two numbers
    ```
    - `Integer`, `Long`, `Float` and `Double` contain this methos.