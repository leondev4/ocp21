---

markmap:
  initialExpandLevel: 1
---
# **Compound Assignment Operators**
- Addition
assignment
  - ```java
    a += 5
    a  = a + 5
    ```
    - Adds the value on the right to the variable on 
    the left and assigns the sum to the variable
      - Subtraction
        assignment
        - ```java
          b -= 0.2
          b  = b-0.2
          ```
          - Subtracts the value on the right from the variable on 
          the left and assigns the difference to the variable
- Multiplication
  assignment
  -   ```java
      c *= 100
      c  = c*100
      ```
      - Multiplies the value on the right with the variable
      on the left and assigns the product to the variable
        - Division
        assignment
          - ```java
            d /= 4
            d  = d/4
            ```
            - Divides the variable on the left by the value on 
            the right and assigns the quotient to the variable
- ```java
  int camel = 2, giraffe = 3;
  camel = camel * giraffe; //Simple assignment operator
  camel *= giraffe;     //Compound assignment operator
  ```
  - The left side of the compound operator can be applied only to a 
  variable that is already defined and cannot be used to declare a 
  new variable. In this example, if `camel` were not already defined, 
  the expression `camel *= giraffe` would not compile.
- **Compound operators are useful for more than just shorthand. 
They can also save you from having to explicitly cast a value.**
  - ```java
    long goat = 10;
    int sheep = 5;
    sheep = sheep * goat; // DOES NOT COMPILE
    ```
    - We are trying to assign a `long` value to an `int` variable. 
    This last line could be fixed with an explicit cast to `(int)`, 
    but there’s a better way using the compound assignment
    operator:
    - ```java
      long goat = 10;
      int sheep = 5;
      sheep *= goat;
      ```
      The compound operator will first cast `sheep` to a `long`, apply the multiplication of 
      two `long` values, and then cast the result to an `int`. Unlike the previous example, 
      in which the compiler reported an error, the compiler will automatically cast the 
      resulting value to the data type of the value on the left side of the compound 
      operator.
- **Return Value of Assignment Operators**
The result of an assignment is an expression in and of itself
equal to the value of the assignment.
  - ```java
    long wolf = 5;
    long coyote = (wolf = 3);
    System.out.println(wolf);   // 3
    System.out.println(coyote); // 3
    ```
    - The key here is that `(wolf=3)` does two things. First, it sets
the value of the variable `wolf` to be `3`. Second, it returns a
value of the assignment, which is also `3`.
  - Don’t be surprised if you see an if statement 
  on the exam similar to the following
    - ```java 
      boolean healthy = false;
        if(healthy = true)
          System.out.print("Good!");
      ```
      - While this may look like a test if `healthy` is
      `true`, it’s actually assigning `healthy` a value
      of `true`. The result of the assignment is the 
      value of the assignment, which is `true`, 
      resulting in this snippet printing `Good!`.