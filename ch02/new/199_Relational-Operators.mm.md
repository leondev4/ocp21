---

markmap:
  initialExpandLevel: 1
---
# **Relational Operators**
- Compare two expressions and return a `boolean` value.
  - `a < 5`
    - Returns `true` if the value on the left is strictly less than the value on the right
  - `b <= 6`
    - Returns `true` if the value on the left is less than or equal to the value on the right
  - `c > 9`
    - Returns `true` if the value on the left is strictly greater than the value on the right
  - `3 >= d`
    - Returns `true` if the value on the left is greater than or equal to the value on the right
  - `e  instanceof String`
    - Returns `true` if the reference on the left side is an instance of the type on the 
    right side (class, interface, record, enum, annotation)
- **Numeric Comparison Operators**
  - The first four relational operators apply only to numeric values. 
  If the two numeric operands are not of the same data type, 
  the smaller one is promoted
    -   ```java
        int gibbonNumFeet = 2, wolfNumFeet = 4,  ostrichNumFeet = 2;
        System.out.println(gibbonNumFeet < wolfNumFeet); // true
        System.out.println(gibbonNumFeet <= wolfNumFeet); // true
        System.out.println(gibbonNumFeet >= ostrichNumFeet); // true
        System.out.println(gibbonNumFeet > ostrichNumFeet); // false
        ```
        - Notice that the last example outputs `false`, because although `gibbonNumFeet` and `ostrichNumFeet`
        have the same value, `gibbonNumFeet` is not strictly greater than `ostrichNumFeet`.
- **`instanceof` Operator**
It is useful for determining whether an arbitrary object is 
a member of a particular class or interface at runtime.
  - Objects can be passed around using a variety of references.
For example, all classes inherit from `java.lang.Object`. This 
means that any instance can be assigned to an `Object` 
reference. For example, how many objects are created and 
used in the following code snippet?
    -   ```java
        Integer zooTime = Integer.valueOf(9);
        Number num = zooTime;
        Object obj = zooTime;
        ```
        - Only one object is created in memory, but there are three different references to it because `Integer` 
        inherits both `Number` and `Object`. This means you can call `instanceof` on any of these references 
        with three different data types, and it will return `true` for each of them.
- Where polymorphism often comes into play is when you create a 
method that takes a data type with many possible subclasses.
  - We want the function to add `O’clock` to the end of
output if the value is a whole number type, such as 
an `Integer`; otherwise, it just prints the value.
    - ```java
      public void openZoo(Number time) {
        if(time instanceof Integer)
          System.out.print((Integer)time + " O'clock");
        else
          System.out.print(time);
      }
      ```
      - We have a method that can intelligently handle both
      `Integer` and other values.
      - Notice that we cast the `Integer` value in this example. It is
      common to use casting with `instanceof` when working 
      with objects that can be various different types, since
      casting gives you access to fields available only in the 
      more specific classes. 
- **Invalid `instanceof`**
`Number` cannot possibly hold a `String` value
  - ```java
    public void openZoo(Number time) {
      if(time instanceof String) //DOES NOT COMPILE
        System.out.print(time);
    }
    ```
    - If the compiler can determine that a variable cannot
possibly be cast to a specific class, it reports an error.
- **`null` and the `instanceof` operator**
  - You should know that calling `instanceof` on the `null` 
  literal or a `null` reference always returns `false`.
    - ```java
      boolean r1 =null instanceof Object;        // false
      String noObjectHere = null;
      boolean r2 =noObjectHere instanceof String;// false
      ```
      - This example does not compile, since `null` is used on the right side of 
      the `instanceof` operator (you must use on the right side a type):
        ```java
        System.out.print(null instanceof null); //DOES NOT COMPILE
        ```