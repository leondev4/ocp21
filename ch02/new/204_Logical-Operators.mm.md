---

markmap:
  initialExpandLevel: 1
---
# **Logical Operators**
- The _logical operators_, (`&`), (`|`), and (`^`), may be applied to both numeric 
and boolean data types. When they’re applied to boolean data types, 
they’re referred to as logical operators. Alternatively, when they’re applied to 
numeric data types, they’re referred to as _bitwise operators_.
  - Logical AND `a & b`
    - The value is `true` only if both values are `true`.
  - Logical inclusive OR `c | d`
    - The value is `true` if at least one of the values is `true`.
  - Logical exclusive OR `e ^ f`
    - The value is `true` only if one value is `true` and the other is `false`.
- Here are some tips to help you remember
  - AND is only `true` if both operands are `true`.
    - ```java
      boolean eyesClosed = true;
      boolean breathingSlowly = true;
      ```
  - Inclusive OR is only `false` if both operands are `false`.
    - ```java
      boolean resting = eyesClosed | breathingSlowly;
      boolean asleep = eyesClosed & breathingSlowly;
      boolean awake = eyesClosed ^ breathingSlowly;
      ```
  - Exclusive OR is only `true` if both operands are different.
    -   ```java
        System.out.println(resting); // true
        System.out.println(asleep); // true
        System.out.println(awake); // false
        ```
- **Bitwise Operators**
Bitwise operations that compare the `0`s and `1`s of a number,
and **return a new number** based on these comparisons. 
  - Bitwise AND `a & b`
    - Compares the bits of two numbers, returning a number that has a `1` in
each digit in which both operands have a `1`, and `0` otherwise.
      - `1 & 1 = 1` otherwise `0`
  - Bitwise OR `c | d`
    - Compares the bits of two numbers, returning a number that has a `1` in
each digit in which either operand has a `1`, and `0` otherwise.
      - `0 | 0 = 0` otherwise `1`
  - Bitwise
exclusive OR `e ^ f`
    - Compares the bits of two numbers, returning a number that has a `0` in
each digit that matched, and `1` otherwise.
      - `0 ^ 0 = 1 ^ 1 = 0` otherwise `1`
- First we have the bitwise AND operator (`&`) and the bitwise 
OR operator (`|`). **You need to know that the original 
number is returned if both operands are the same.**
  - ```java
    int number = 70;
    System.out.println(number); //70
    System.out.println(number&number); //70
    System.out.println(number|number); //70
    ```
    - **You also need to know how bitwise operations work on a number and its negation, 
    which returns `0` for bitwise AND (`&`), and `-1` for bitwise OR (`|`).**
      - ```java
        int negated = ~number;
        System.out.println(negated); // -71

        System.out.println(number&negated); // 0
        System.out.println(number|negated); // -1
        ```
- Finally, we have the binary exclusive OR operator (`^`). It works like 
the boolean version except with `0` as `false`, and `1` as `true`. 
  - **You should know that it returns `0` if both numbers 
  are the same, and `-1` for a value with its negation.**
    - ```java
      System.out.println(number ^ number); // 0
      System.out.println(number ^ negated); // -1
      ```
- **In summary**
  - ```java
    num & num=1
    num | num=1
    ```
    - ```
      num & numNeg=0
      num | numNeg=-1
      ```
      -   ```java
          num ^ num=0
          num ^ numNeg=-1
          ```