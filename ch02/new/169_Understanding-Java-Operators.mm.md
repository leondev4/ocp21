---

markmap:
  initialExpandLevel: 1
---
# **Understanding Java Operators**
- A _Java operator_ is a special symbol that can be applied to a set 
of variables, values, or literals—referred to as operands—and 
that returns a result. The term _operand_, which we use throughout this 
chapter, refers to the value or variable the operator is being applied  
  - The output of the operation is simply referred to as the result.
    - ```java
      var c = a + b;
      ```
      - `a` and `b` are operands
      - `+` (plus) is the operator.
      - Result assigned to `c`
      - It contains a second operation, with the assignment operator (`=`) 
      being used to store the result in variable `c`.
  - Java supports three types of operators: unary, binary, and ternary. 
  These types of operators can be applied to one, two, or three 
  operands, respectively.
    - Java operators are not necessarily evaluated from left-to-right 
    order. In this following example, the second expression is 
    actually evaluated from right to left, given the specific operators 
    involved:
      - ```java
        int cookies = 4;
        double reward = 3 + 2 * --cookies;
        var s = "Zoo animal receives: "+reward+" reward points";
        ```
        - `s` contains
            ```java
            Zoo animal receives: 9.0 reward points
            ```
        - You first decrement `cookies` to `3`, then multiply the resulting value by 
        `2`, and finally add `3`. **The value then is automatically promoted 
        from 9 to 9.0 and assigned to `reward`**. The final values of `reward` 
        and `cookies` are 9.0 and 3, respectively.
- Determining which operators are evaluated in what order is referred 
to as _operator precedence_.
  - Consider the following expression:
    ```java
    var perimeter = 2 * height + 2 * length;
    ```
    Let’s apply some optional parentheses to demonstrate how the 
    compiler evaluates this statement:
    ```java
    var perimeter = ((2 * height) + (2 * length));
    ```
      - The multiplication operator (`*`) has a higher precedence than the 
      addition operator (`+`), so the `height` and `length` are both 
      multiplied by `2` before being added together. The assignment 
      operator (`=`) has the lowest order of precedence, so the 
      assignment to the `perimeter` variable is performed last.
      - Unless overridden with parentheses, Java operators follow order of operation, 
      listed in the [Table 2.1](https://1drv.ms/i/c/c83cfca51d5c2032/EVvIozQUveROs14p0w2GV-kBSUvvsxsaMgeGsv17I2J0Bg?e=gPOAKq), by decreasing order of operator precedence. If two 
      operators have the same level of precedence, then Java guarantees left-to-right
      evaluation for most operators other than the ones marked in the table.