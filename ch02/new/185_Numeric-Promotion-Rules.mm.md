---

markmap:
  initialExpandLevel: 1
---
# **Numeric Promotion Rules**
- 1.- If two values have different data types, Java will automatically 
promote one of the values to the larger of the two data types.
2.- If one of the values is integral and the other is floating-point,
Java will automatically promote the integral value to the floating-
point value’s data type.
  - 3.- `byte`, `short`, and `char`, are first promoted to `int` any 
  time they’re**used with a binary arithmetic operator with 
  a variable**(as opposed to a value), even if neither of the
  operands is `int`.
  4.- After all promotion has occurred and the operands have
  the same data type, the resulting value will have the
  same data type.
    - Rule 3
      ```java
      byte i=10;
      i=i+1; //DOES NOT COMPILE
      i=1+2; //COMPILES
      ```
    - For the **third rule, note that unary increment/decrement 
    operators are excluded.** For example, applying `++` to a
    `short` value results in a `short` value.
- What is the data type of `x * y`?
  ```java
  int x = 1;
  long y = 33;
  var z = x * y;
  ```
  - We follow the first rule. Since one of the values is `int` 
  and the other is `long` and since `long` is larger than
  `int`, the `int` value `x` is first promoted to a `long`.
  The result `z` is then a `long` value.
    - What is the data type of `x + y`?
      ```java
      double x = 39.21;
      float y = 2.1;
      var z = x + y;
      ```
      - The second line does not compile! As you may remember
      floating-point literals are assumed to be `double` unless 
      postfixed with an `f`, as in `2.1f`. If the value of `y` was set 
      properly to `2.1f`, then the promotion would be similar to 
      the previous example, with both operands being promoted
      to a `double`, and the result `z` would be a `double` value.
- What is the data type of x * y?
  ```java
  short x = 10;
  short y = 3;
  var z = x * y;
  ```
  - On the last line, we must apply the third rule: that `x` and `y` 
  will both be promoted to `int` before the binary multiplication 
  operation, resulting in an output of type `int`. If you were to 
  try to assign the value to a `short` variable `z` without casting, 
  then the code would not compile. Pay close attention to the
  fact that the resulting output is not a `short`.
    - What is the data type of `w * x / y`?
      ```java
      short w = 14;
      float x = 13;
      double y = 30;
      var z = w * x / y;
      ```
      - We must apply all of the rules. First, `w` will automatically be 
      promoted to `int` solely because it is a `short` and is being 
      used in an arithmetic binary operation. The promoted `w` 
      value will then be automatically promoted to a `float` so that 
      it can be multiplied with `x`. The result of `w * x` will then be 
      automatically promoted to a `double` so that it can be divided 
      by `y`, resulting in a `double` value.
- **Assignment Operator**
- An _assignment operator_ is a binary operator that modifies,
or assigns, the variable on the left side of the operator with
the result of the value on the right side of the equation.
  - The assignment operator is evaluated from right to left.
    - ```java
      int herd = 1;
      ```
      - This statement assigns the `herd` variable the value of `1`.
      - Assigns the value on the right to the variable on the left
  - Java will automatically promote from smaller to larger data types,
  but it will throw a compiler error if it detects that you are trying 
  to convert from larger to smaller data types without casting.