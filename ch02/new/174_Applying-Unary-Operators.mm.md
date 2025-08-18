---

markmap:
  initialExpandLevel: 1
---
# **Applying Unary Operators**
- A unary operator is one that requires exactly one operand, 
or variable, to function. They often perform simple tasks, 
such as increasing a numeric variable by one or negating a 
`boolean` value.
  - The precedence is:
    - Post-unary operators
      - Post-increment `y++`
        - Increases the value by 1 and returns the _original_ value
      - Post-decrement `z--`
        - Decreases the value by 1 and returns the _original_ value
    - Pre-unary operators
      - Pre-increment `++w`
        - Increases the value by 1 and returns the _new_ value
      - Pre-decrement `--x`
        - Decreases the value by 1 and returns the _new_ value
    - Other unary operators
      - Logical complement `!a`
        - Inverts a `boolean`’s logical value
      - Bitwise complement `~b`
        - Inverts all `0`s and `1`s in a number
      - Plus `+c`
        - Indicates a number is positive; is same as `c`
      - Negation or minus `-d`
        - Indicates a literal number is negative or negates an expression
    - Cast
      - `(String)i`
        - Casts a value to a specific type
- **Increment and decrement operators**, (`++`) and (`--`),
respectively, can be applied to numeric variables and have a
high order of precedence compared to binary operators. In
other words, they are often applied first in an expression.
  - Increment and decrement operators require special 
  care because the order in which they are attached to
  their associated variable can make a difference in 
  how an expression is processed.
    - The following code snippet illustrates this distinction:
      ```java
      int parkAttendance = 0;
      System.out.println(parkAttendance);     //0
      System.out.println(++parkAttendance);   //1
      System.out.println(parkAttendance);     //1
      System.out.println(parkAttendance--);   //1
      System.out.println(parkAttendance);     //0
      ```
      - The first pre-increment operator updates the value for
      `parkAttendance` and outputs the new value of 1. The next post-
      decrement operator also updates the value of `parkAttendance`
      but outputs the value before the decrement occurs.
      - For the exam, it is critical that you know the difference between 
      expressions like `parkAttendance++` and `++parkAttendance`.
- **Complement and Negation Operators**
The _logical complement operator (`!`)_ flips the value of a
`boolean` expression. For example, if the value is `true`,
it will be converted to `false`, and vice versa.
  - ```java
    boolean isAnimalAsleep = false;
    System.out.print(isAnimalAsleep); //false
    isAnimalAsleep = !isAnimalAsleep;
    System.out.print(isAnimalAsleep); //true
    ```
    - The _bitwise negation operator (`~`)_ turns all the zeros into ones 
    and vice versa.**You can figure out the new value by
    negating the original and subtracting one.**
      - ```java
        int number = 70;
        int negated = ~number;
        System.out.println(negated);  // -71
        System.out.println(~negated); // 70
        ```
  - The _negation operator (`-`)_ reverses the sign of a
  numeric expression
    - ```java
      double zooTemperature = 1.21;
      System.out.println(zooTemperature); // 1.21
      zooTemperature = -zooTemperature;
      System.out.println(zooTemperature); // -1.21
      zooTemperature = -(-zooTemperature);
      System.out.println(zooTemperature); // -1.21
      ```
        - Notice that in the previous example we used parentheses,
        `()`, for the negation operator, `-`, to apply the negation twice.
        If we had instead written `--`, then it would have been
        interpreted as the decrement operator and printed `-2.21`.
  - Based on the description, it might be obvious that some
operators require the variable or expression they’re acting
on to be of a specific type. For example, you cannot apply a
negation operator (`-`) to a `boolean` expression, nor can you
apply a logical complement operator (`!`) to a numeric
expression. The code fails to compile.
    - None of the following lines of code will compile:
      ```java
      int pelican = !5;        // DOES NOT COMPILE
      boolean penguin = -true; // DOES NOT COMPILE
      boolean parrot = ~true;  // DOES NOT COMPILE
      boolean peacock = !0;    // DOES NOT COMPILE
      ```
      - The first statement will not compile because in Java you cannot 
      perform a logical inversion of a numeric value. The second and 
      third statements do not compile because you cannot numerically 
      negate or complement a `boolean` value; you need to use the 
      logical inverse operator. Finally, the last statement does not 
      compile because you cannot take the logical complement of 
      a numeric value, nor can you assign an integer to a `boolean` 
      variable.