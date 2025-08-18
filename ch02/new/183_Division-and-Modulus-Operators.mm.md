---

markmap:
  initialExpandLevel: 1
---
# **Division and Modulus Operators**
- The _modulus operator_, sometimes called the _remainder operator_, 
is simply the remainder when two numbers are divided.
  - ```java
    System.out.println(9 / 3); //3
    System.out.println(9 % 3); //0

    System.out.println(10 / 3); //3
    System.out.println(10 % 3); //1

    System.out.println(11 / 3); //3
    System.out.println(11 % 3); //2

    System.out.println(12 / 3); //4
    System.out.println(12 % 3); //0
    ```
    - For a given divisor `y`, the modulus operation results in a value between 
    `0` and (`y - 1`) for positive dividends, or `0`, `1`, `2` in this example.
    - For integer values, division results in the floor value of the nearest integer 
    that fulfills the operation, whereas modulus is the remainder value. 
    - You just take the value before the decimal point,
     regardless of what is after the decimal point.
    - For example, the floor value is `4` for each of the values `4.0`, `4.5`, and `4.9999999`.
- You can also use modulus with negative numbers. If the
divisor is negative, then the negative sign is ignored.
Negative values do change the behavior of modulus when
applied to the dividend
  - ```java
    System.out.println(2 % 5); // 2
    System.out.println(7 % 5); // 2
    System.out.println(2 % -5); // 2
    System.out.println(7 % -5); // 2

    System.out.println(-2 % 5); // -2
    System.out.println(-7 % 5); // -2
    System.out.println(-2 % -5); // -2
    System.out.println(-7 % -5); // -2
    ```
    - If the numerator is negative then the remainder will be negative 
    otherwise positive. The denominator sign doesn't matter. In the 
    expression `a/b`; `a` is numerator and `b` is denominator.