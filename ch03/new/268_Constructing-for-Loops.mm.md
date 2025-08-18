---

markmap:
  initialExpandLevel: 1
---

# **Constructing for Loops**
- There are two types of `for` loops, although both use the same `for` keyword. 
The first is referred to as the basic `for` loop, and the second is often called 
the enhanced `for` loop. For clarity, we refer to them as the `for` loop and the 
for-each loop, respectively,
- **The `for` Loop**
A basic `for` loop has the same termination condition expression as the `while` 
loops, as well as two new sections: an initialization block and an update 
statement (See [Figure 3.10](https://1drv.ms/i/c/c83cfca51d5c2032/EYSntlm-p9tNtia-4Epu0sUBnTXgyEUB9jgTRZPR4vR5ow?e=rThcbf)).
  - Each of the three sections is separated by a semicolon. In addition, 
  the initialization and update sections may contain multiple statements, 
  separated by commas.
    - Variables declared in the initialization block of a `for` loop have limited scope 
    and are accessible only within the `for` loop.  For example, this code does not 
    compile because the loop variable `i` is referenced outside the loop:
      - ```java
        for( int i=0; i < 10; i++)
          System.out.println("Value is: "+i);
        System.out.println(i); // DOES NOT COMPILE
        ```
        - Variables declared before the `for` loop and assigned a value in the initialization block may be
        used outside the `for` loop because their scope precedes the creation of the `for` loop.
          - ```java
            int i;
            for(i = 0; i < 10; i++)
              System.out.println("Value is: "+i);
            System.out.println(i);
            ```
- Let’s take a look at an example that prints the first five
numbers, starting with zero:
  - ```java
    for(int i = 0; i < 5; i++){
      System.out.print(i + " ");
    }
    ```
    - The local variable `i` is initialized first to `0`. The variable `i` is only in scope 
    for the duration of the loop and is not available outside the loop once the 
    loop has completed. Like a `while` loop, the `boolean` condition is evaluated 
    on every iteration of the loop before the loop executes. Since it returns 
    `true`, the loop executes and outputs `0` followed by a space. Next, the loop 
    executes the update section, which in this case increases the value of `i` to 
    `1`. The loop then evaluates the `boolean` expression a second time, and the 
    process repeats multiple times, printing the following:  `0 1 2 3 4`
      - On the fifth iteration of the loop, the value of `i` reaches `4` and is incremented 
      by `1` to reach `5`. On the sixth iteration of the loop, the `boolean` expression 
      is evaluated, and since `(5 <5)` returns `false`, the loop terminates without 
      executing the statement loop body.
- **Printing Elements in Reverse**
The goal is to print `4 3 2 1 0`.  An initial implementation
 might look like the following:
  - ```java
    for (var counter = 5; counter > 0; counter--){
      System.out.print(counter + " ");
    }
    ```
    This is the output:
    `5 4 3 2 1`
    - Let’s fix that by starting with `4` instead:
      ```java
      for(var counter = 4; counter> 0; counter--) {
        System.out.print(counter + " ");
      }
      ```
      It prints the following:
      `4 3 2 1`
      - We need to update the termination condition, like this:
        ```java
        for(var counter = 4; counter >= 0; counter--) {
          System.out.print(counter + " ");
        }
        ```
        - Finally! We have code that now prints `4 3 2 1 0`. 
        We could have instead used `counter > -1` as the 
        loop termination condition in this example, 
        although `counter >= 0` tends to be more readable.
          - If you do see a `for` loop with a decrement operator 
          on the exam, you should assume they are trying to
           test your knowledge of loop operations.
- 1.-  **Creating an Infinite Loop**
  ```java
  for ( ; ; )
   System.out.println("Hello World");
  ```
  - Although this `for` loop may look like it does not compile, it will in fact compile
   and run without issue. It is actually an infinite loop that will print the same 
   statement repeatedly. This example reinforces the fact that the components of 
   the `for` loop are each optional. Note that the semicolons separating the three 
   sections are required, as `for()` without any semicolons will not compile.
    - 2.- **Adding Multiple Terms to the for Statement**
      ```java
      int x = 0;
      for(long y = 0, z = 4; x < 5 && y < 10; x++, y++){
        System.out.print(y + " "); } //0 1 2 3 4
      System.out.print(x + " ");//5
      ```
      - This code demonstrates three variations of the `for` loop you may not have seen. 
      First, you can declare a variable, such as `x` in this example, before the loop 
      begins and use it after it completes. Second, your initialization block, `boolean` 
      expression, and update statements can include extra variables that may or may 
      not reference each other. For example, `z` is defined in the initialization block
      and is never used. Finally, the update statement can modify multiple variables.
        - 3.- **Redeclaring a Variable in the Initialization Block**
          ```java
          int x = 0;
          for(int x = 4; x < 5; x++) // DOES NOT COMPILE
            System.out.print(x + " ");
          ```
          - We can fix this loop by removing the declaration of `x` from the for loop as follows:
        - This example looks similar to the previous one, but it does not compile 
        because of the initialization block. The difference is that the declaration 
        of `x` is repeated in the initialization block after already being declared
        before the loop, resulting in the compiler stopping because of a 
        duplicate variable declaration.
          - ```java
            int x = 0;
            for( ; x < 5; x++)
              System.out.print(x+" ");
            ```
- 4.- **Using Incompatible Data Types in the Initialization Block**
  ```java
  int x = 0;
  for(long y=0,int z=4;x<5; x++) // DOES NOT COMPILE
    System.out.print(y + " ");
  ```
  - This code will not compile. The variables in the initialization block must all 
  be of the same type. In the second example, `y` and `z` were both `long`, so 
  the code compiled without issue; but in this example, they have different types, 
  so the code will not compile.
    - 5.- **Using Loop Variables Outside the Loop**
        ```java
        for (long y = 0,x = 4; x < 5 && y < 10; x++, y++)
          System.out.print(y + " ");
        System.out.print(x); // DOES NOT COMPILE
        ```
        -  If you notice, `x` is defined in the initialization block of the loop and then 
        used after the loop terminates. Since `x` was scoped only for the loop, 
        using it outside the loop will cause a compiler error.
            - **Modifying Loop Variables**
              As a general rule, it is considered a poor coding practice
              to modify loop variables as it can lead to an unexpected
              result
              - ```java
                for(int i=0; i<10; i++) //Infinite Loop
                  i = 0;

                for(int j=1; j<10; j++) // Iterates 5 times
                  j++;
                ```