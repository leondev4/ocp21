---

markmap:
  initialExpandLevel: 1
---
# **The `if` Statement**
- Control flow statements break up the flow of execution by
using decision-making, looping, and branching, allowing
the application to selectively execute particular segments
of code.
  - These statements can be applied to single expressions as
well as a block of Java code.
    - ```java
      // Single statement
      patrons++;
      ```
      - A statement or block often serves as the target of a
decision-making statement.
        - ```java
          // Single statement
          if (ticketsTaken> 1)
            patrons++;
          ```
        - ```java
          // Statement inside a block
          if (ticketsTaken> 1) {
            patrons++;
          }
          ```
    - ```java
      // Statement inside a block
      {
        patrons++;
      }
      ```
- Allows our application to execute a particular block of
code if and only if a `boolean` expression evaluates 
to `true` at runtime. (see [Figure 3.1](https://1drv.ms/i/c/c83cfca51d5c2032/ET6zVaABj-5DsyXLmC_qvQsBPk1QZyfJhYdGp6CvQLfsbQ?e=eQ0SXl))
  - Imagine we had a function that used the hour
of day, an integer value from `0` to `23`, to 
display a message to the user:
    - ```java
      if(hourOfDay<11)
        System.out.println("Good Morning");
      ```
      - Now let’s say we also wanted to increment some value, `morningGreetingCount`, 
      every time the greeting is printed.
      - We could write the `if` statement twice, but luckily
Java offers us a more natural approach using a block:
        - ```java
          if(hourOfDay<11) {
            System.out.println("Good Morning");
            morningGreetingCount++;
          }
          ```
        - **Watch Indentation and Braces**
        - For example, take a look at this slightly modified form of our example:
          ```java
          if(hourOfDay < 11)
            System.out.println("Good Morning");
            morningGreetingCount++;
          ```
          Based on the indentation, you might be inclined to think the variable `morningGreetingCount` is only
          going to be incremented if `hourOfDay` is less than `11`, but that’s not what this code does. It will
          execute the print statement only if the condition is met, but it will always execute the increment
           operation.
    - If the hour of the day is less than `11`, 
    then the message will be displayed.
- **The `else` Statement**
What if we want to display a different 
message if it is 11 a.m. or later?
  - ```java
    if(hourOfDay < 11) {
      System.out.println("Good Morning");
    }
    if(hourOfDay>= 11) {
        System.out.println("Good Afternoon");
    }
    ```
    - This seems a bit redundant, though, since we’re
    performing an evaluation on `hourOfDay` twice. 
    Luckily, Java offers us a more useful approach 
    in the form of an `else` statement (see [Figure 3.2](https://1drv.ms/i/c/c83cfca51d5c2032/ERipQNuatIRMoObzWJFIjSEBJ3xvCMbnHw4nj2n0OHeD9A?e=Apvbv3))
      - Example:
        ```java
        if(hourOfDay < 11){
          System.out.println("Good Morning");
        }else System.out.println("Good Afternoon");
        ```
        - Now our code is truly branching between one of the two
        possible options, with the `boolean` evaluation happening
        only once. The `else` operator takes a statement or block of
        statements, in the same manner as the `if` statement.
- Similarly, we can append additional `if` statements 
to an `else` block to arrive at a more refined example:
  - ```java
    if(hourOfDay < 11){
      System.out.println("Good Morning");
    }else if(hourOfDay < 15){
      System.out.println("Good Afternoon");
    }else { System.out.println("Good Evening");
    }
    ```
    - The Java process will continue execution until it encounters an `if` statement 
    that evaluates to `true`. If neither of the first two expressions is `true`, it will 
    execute the code of the final `else` block.
      - **Verifying That the `if` Statement Evaluates to a
      Boolean Expression**
        ```java
        int hourOfDay = 1;
        if(hourOfDay){ // DOES NOT COMPILE
          …
        }
        ```
        - This statement is not valid in Java.