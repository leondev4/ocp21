---

markmap:
  initialExpandLevel: 1
---

# **Working with Binary <br/>Arithmetic Operators**
-  They take two. Arithmetic operators are those 
that operate on numeric values. Arithmetic 
operators also include the unary operators, `++` 
and `--`, which we covered already.
    - The precedence is:
      - Multiplication/division/modulus
        - Multiplication `e * f` 
          - Multiplies two numeric values
        - Division `g/h`
          - Divides one numeric value by another
        - Modulus `i%j`
          - Returns the remainder after division of one numeric value by another
      - Addition/subtraction
        - Addition `a+b`
          - Adds two numeric values
        - Subtraction `c-d`
          - Subtracts two numeric values
    - All of the arithmetic operators may be applied to any Java primitives, with the 
    exception of `boolean`. Furthermore, only the addition operators `+` and `+=` 
    may be applied to `String` values, which results in `String` concatenation.
- The _multiplicative operators_ (`*`,` /`, `%`) have 
a higher order of precedence than the 
_additive_ operators (`+`, `-`).
  - Take a look at the following expression:
    ```java
    int price = 2 * 5 + 3 * 4 - 8;
    ```
      - First, you evaluate the `2 * 5` and `3 * 4`, 
      which reduces the expression to this:
        ```java
        int price = 10 + 12 - 8;
        ```
        - Then, you evaluate the remaining terms in
        left-to-right order, resulting in a value of 
        `price` being `14`.
- **Adding Parentheses**
You can change the order of operation explicitly by wrappin
parentheses around the sections you want evaluated first.
  ```java
  int price = 2 * ((5 + 3) * 4 - 8);
  ```
  - This time you would evaluate the addition operator `5 + 3`,
  which reduces the expression to the following:
    ```java
    int price = 2 * (8 * 4 - 8);
    ```
    - You can further reduce this expression by multiplying
    the first two values within the parentheses.
      ```java
      int price = 2 * (32 - 8);
      ```
      - Next, you subtract the values within the parentheses
      before applying terms outside the parentheses.
        ```java
        int price = 2 * 24;
        ```
        Finally, you would multiply the result by `2`, resulting 
        in a value of `48` for price.
- When working with parentheses, you need to make sure
they are always valid and balanced.
  - ```java
    long pigeon = 1 + ((3 * 5) / 3; // DOES NOT COMPILE
    int blueJay = (9 + 2) + 3) / (2 * 4; // DOES NOT COMPILE
    ```
    - The first example does not compile because the
parentheses are not balanced.
    - The second example has an equal number of left and right 
    parentheses, but they are not balanced properly.
  - Let’s try another example:
    ```java
    short robin = 3 + [(4 * 2) + 4]; // DOES NOT COMPILE
    ```
    -  Java does not allow brackets, `[]`, to be used in place of 
    parentheses. If you replace the brackets with parentheses,
    the example will compile just fine.