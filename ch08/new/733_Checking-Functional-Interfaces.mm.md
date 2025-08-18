---

markmap:
  initialExpandLevel: 1
---
# **Checking Functional<br/> Interfaces**
- What functional interface would you use in these three situations?
  - Returns a `String` without taking any parameters
    - `Supplier<String>`
  - Returns a `Boolean` and takes a `String`
    - `Function<String,Boolean>`
      You might think it is a `Predicate<String>`. Note that a `Predicate` 
      returns a `boolean` primitive and not a `Boolean` object.
  - Returns an `Integer` and takes two `Integers`
    - `BinaryOperator<Integer>` or a `BiFunction<Integer,Integer,Integer>`.
`BinaryOperator<Integer>` is the better answer of the two since it is more specific.
- What functional interface would you use to fill in the blanks for these?
  - ```java
    6: ______ <List> ex1 = x -> "".equals(x.get(0));
    ```
    - `Predicate`
  - ```java
    7: ______ <Long> ex2 = (Long l) -> System.out.println(l);
    ```
    - `Consumer`
  - ```java
    8: ______ <String, String> ex3 = (s1, s2) -­> false;
    ```
    - `BiPredicate`
- __*identify the error.*__
  - ```java
    6: Function<List<String>> ex1 = x-> x.get(0);//DOES NOT COMPILE
    ```
    - Claims to be a `Function`. A `Function` needs to specify two generic types: the
    input parameter type and the return value type. The return value type is missing, 
    causing the code not to compile.
  - ```java
    7: UnaryOperator<Long> ex2 = (Long l) -­>3.14;//DOES NOT COMPILE
    ```
    - `UnaryOperator`, which returns the same type as it is passed in. The example returns 
    a `double` rather than a `Long`, causing the code not to compile.