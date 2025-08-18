---

markmap:
  initialExpandLevel: 1
---

# **The `return` Statement**
- For now, though, you should be familiar with the idea that
creating methods and using `return` statements can be used 
as an alternative to using labels and `break` statements. 
  - ```java
    public class FindInMatrixUsingReturn {
      private static int[] searchForValue(int[][] list, int v) {
        for (int i = 0; i < list.length; i++) {
          for (int j = 0; j < list[i].length; j++) {
            if (list[i][j] == v) {
              return new int[]{i, j};
            }
          }
        }
        return null;
      }

      public static void main(String[] args) {
        int[][] list = {{1, 13}, {5, 2}, {2, 2}};
        int searchValue = 2;
        int[] results = searchForValue(list, searchValue);
        if (results == null) {
          System.out.print("Value " + searchValue + " not found");
        } else {
          System.out.print("Value " + searchValue + " found at:" +
                    "(" + results[0] + "," + results[1] + ")" );
        }
    }
    }
    ```
    - This class is functionally the same as the first `FindInMatrix` class we saw earlier using `break`. 
    If you need finer-grained control of the loop with multiple `break` and `continue` statements, 
    the first class is probably better. That said, we find code without labels and `break` statements
    a lot easier to read and debug. Also, making the search logic an independent function makes 
    the code more reusable and the calling `main()` method a lot easier to read.
    - Just remember that `return` statements can be used to exit loops quickly and can lead to more 
    readable code in practice, especially when used with nested loops.
- **Unreachable Code**
One facet of `break`, `continue`,  `return` and `yield` that you should be aware of is that 
**any code placed immediately after them in the same block is considered 
unreachable and will not compile.**
  - ```java
    int checkDate = 0;
    while (checkDate<10) {
      checkDate++;
      if(checkDate>100) {
        break;
        checkDate++; // DOES NOT COMPILE
      }
    }
    ```
    - Even though it is not logically possible for the `if` statement
to evaluate to `true` in this code sample, the compiler notices
that you have statements immediately following the `break`
and will fail to compile with “unreachable code” as the reason. 
The same is true for `continue` and `return` statements, as
 shown in the following two examples:
      - ```java
        int minute = 1;
        WATCH: while (minute>2) {
          if (minute++>2) {
            continue WATCH;
            System.out.print(minute); // DOES NOT COMPILE
          }
        }

        int hour = 2;
        switch (hour) {
          case 1: return; hour++; // DOES NOT COMPILE
          case 2:
        }
        ```
        - One thing to remember is that it does not matter if the loop or decision structure actually visits the 
        line of code. For example, the loop could execute zero or infinite times at runtime. Regardless 
        of execution, the compiler will report an error if it finds any code it deems unreachable, in this 
        case any statements immediately following a `break`, `continue`, `return` or `yield` statement.
- **TIP**
Some of the most time-consuming questions on the exam could involve nested loops 
with lots of branching. Unless you can spot a compiler error right away, you might 
consider skipping these questions and coming back to them at the end.
  - **Supported control statement features**
    - `while`, `do/while`, `for`
      - Labels, `break`, `continue`
      - They do not support `yield` and `when`
    - `switch`
      - Labels, `break`, `yield`, `when`
      - It does not support `continue`