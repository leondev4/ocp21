---

markmap:
  initialExpandLevel: 1
---

# **The `break` Statement**
- As you saw when working with `switch` statements, a `break`
statement transfers the flow of control out to the enclosing
statement. The same holds true for a `break` statement that 
appears inside of a `while`, `do/while`, or `for` loop, as it will 
end the loop early, as shown in [Figure 3.12](https://1drv.ms/i/c/c83cfca51d5c2032/ES2cgNu4yTBOmDIlGLNwOkIBHZSoGHHf6PwrqlpFPiMyhw?e=v4DHcn)
  - Notice in Figure that the `break` statement can take an
optional label parameter.**Without a label parameter, 
the `break` statement will terminate the nearest inner 
loop it is currently in the process of executing. The 
optional label parameter allows us to break out of 
a higher-level outer loop.**
    - ```java
      10: public class FindInMatrix {
      11:   public static void main(String[] args) {
      12:     int[][] list = {{1,13}, {5,2}, {2,2}};
      13:     int searchValue = 2;
      14:     int positionX = -1;
      15:     int positionY = -1;
      16:
      17:   PARENT_LOOP:for (int i = 0; i < list.length; i++) {
      18:       for (int j = 0; j < list[i].length; j++) {
      19:         if (list[i][j] == searchValue) {
      20:           positionX = i;
      21:           positionY = j;
      22:           break PARENT_LOOP;
      23:         }
      24:       }
      25:      }
      26:     if (positionX == -1 || positionY == -1) {
      27:       System.out.print("Value "+searchValue+" not found");
      28:     } else {
      29:       System.out.print("Value "+searchValue+" found at: " +
      30:           "("+positionX+","+positionY+")");
      31:     }
      32:  } }
      ```
      - When executed, this code will output the following:
      `Value 2 found at: (1,1)`
      In particular, take a look at the statement **`break PARENT_LOOP`**.
      This statement will break out of the entire loop structure as soon 
      as the first matching value is found.
        - Now, imagine what would happen if we replaced the
        body of the inner loop with the following:
          ```java
            19:         if (list[i][j]==searchValue){
            20:           positionX = i;
            21:           positionY = j;
            22:           break;
            23:         }
          ```
          - How would this change our flow, and would the output
          change? Instead of exiting when the first matching value is
          found, the program would now only exit the inner loop
          when the condition was met. In other words, the structure
          would find the first matching value of the last inner loop to
          contain the value, resulting in the following output:
          `Value 2 found at: (2,0)`
        - **break PARENT_LOOP;**: breaks the loop that has the label.
It is like `return` without ends the method.

          

      - Finally, what if we removed the break altogether?
        ```java
        19:       if(list[i][j]==searchValue) {
        20:         positionX = i;
        21:         positionY = j;
        22:
        23:        }
        ```
          - In this case, the code would search for the last value in the
entire structure that had the matching value. The output
would look like this:
`Value 2 found at: (2,1)`
- **The `continue` Statement**
A statement that causes flow to finish the execution 
of the current loop iteration. See [Figure 3.13](https://1drv.ms/i/c/c83cfca51d5c2032/EWkNCVJxDk1GuHptYY7yyBUBVKKn5FClvLn2ch9iZKMWIA?e=YwaRn4)
  - You may notice that the syntax of the `continue` statement mirrors 
  that of the `break` statement. In fact, the statements are identical 
  in how they are used, but with different results. While the `break` 
  statement transfers control to the enclosing statement, the `continue` 
  statement transfers control to the `boolean` expression that 
  determines if the loop should continue. In other words, it ends the 
  current iteration of the loop. Also, like the `break` statement, the
  `continue` statement is applied to the nearest inner loop under 
  execution, using optional label statements to override this behavior.
    - Imagine we have a zookeeper who is supposed to clean the first 
    leopard in each of four stables but skip stable `b` entirely.
      - ```java
        1: public class CleaningSchedule {
        2:   public static void main(String[] args) {
        3:    CLEANING: for (char stables = 'a'; stables<='d'; stables++) {
        4:       for (int leopard = 1; leopard <= 3; leopard++) {
        5:         if (stables=='b' || leopard==2) {
        6:          continue CLEANING;
        7:         }
        8:        System.out.println("Cleaning: "+stables+","+leopard);
        9: } } } }
        ```
        - The loop will return control to the parent loop any time the first value 
        is `b` or the second value is `2`. On the first, third, and fourth executions 
        of the outer loop, the inner loop prints a statement exactly once and then 
        exits on the next inner loop when leopard is `2`. On the second execution 
        of the outer loop, the inner loop immediately exits without printing anything 
        since `b` is encountered right away. 
          -  The following is printed:
              ```java
                Cleaning: a,1
                Cleaning: c,1
                Cleaning: d,1
              ```
        -  **continue CLEANING;**: ends the current execution of the loop and goes to the 
        `boolean` expression of the loop that has the label. It is like `break` without label.
- Now, imagine we remove the `CLEANING` label in the 
`continue` statement so that control is returned to the 
inner loop instead of the outer. Line 6 becomes the 
following:
  ```java
  6:        continue;
  ```
  - This corresponds to the zookeeper cleaning all leopards
except those labeled 2 or in stable b. The output is then the
following:
    ```java
    Cleaning: a,1
    Cleaning: a,3
    Cleaning: c,1
    Cleaning: c,3
    Cleaning: d,1
    Cleaning: d,2
    ```
    - Finally, if we remove the `continue` statement and the
associated if statement altogether by removing lines `5–7`,
we arrive at a structure that outputs all the values, such as
this:
      - ```java
        Cleaning: a,1
        Cleaning: a,2
        Cleaning: a,3
        Cleaning: b,1
        Cleaning: b,2
        Cleaning: b,3
        Cleaning: c,1
        Cleaning: c,2
        Cleaning: c,3
        Cleaning: d,1
        Cleaning: d,2
        Cleaning: d,3
        ```