---

markmap:
  initialExpandLevel: 1
---
# **Making Decisions with the Ternary Operator**
- The conditional operator, (`? : `), otherwise known as the ternary operator,  is the only 
operator that takes three operands. The ternary operator has the following form: 
  - `booleanExpression ? expression1 : expression2` <br/>
    The number of times `booleanExp` appers indicates 
    the number of `?` and `:`.
    - The first operand must be a `boolean` expression, and the
second and third operands can be any expression that
returns a value. The ternary operation is really a condensed
form of a combined `if` and `else` statement that returns a value. 
      - ```java
        int owl = 5;
        int food;
        if(owl < 2) {
          food = 3;
        } else {
          food = 4;
        }
        System.out.println(food); // 4
        ```
    - Compare the previous code snippet with the following
ternary operator code snippet:
      - ```java
        int owl = 5;
        int food = owl < 2 ? 3 : 4;
        System.out.println(food); // 4
        ```
- Note that it is often helpful for readability to add parentheses around the expressions 
in ternary operations, although doing so is certainly not required. 
  - Consider the following two equivalent expressions:
    ```java
    int food1 = owl<4 ? owl>2 ? 3 : 4 : 5;
    int food2 = (owl<4 ? ((owl>2) ? 3 : 4) : 5);
    ```
    - You should know that there is no requirement that second 
    and third expressions in ternary operations have the 
    same data types.
      - Compare the two statements following the variable declaration:
      - ```java
        int stripes = 7;
        System.out.print((stripes> 5) ? 21 : "Zebra");
        int animal = (stripes < 9) ? 3 : "Horse"; //DOES NOT COMPILE
        ```
      - Both expressions evaluate similar boolean values and return an `int` and a 
      `String`, although only the first one will compile. `System.out.print()` does
      not care that the expressions are completely different types, because it can 
      convert both to `Object` values and call `toString()` on them. On the other 
      hand, the compiler does know that `"Horse"` is of the wrong data type and 
      cannot be assigned to an `int`; therefore, it does not allow the code to be 
      compiled.
- **Ternary Expression and Unperformed Side Effects**
A ternary expression can contain an unperformed side effect, as only one of the 
expressions on the right side will be evaluated at runtime.
  - ```java
    int sheep = 1;
    int zzz = 1;
    int sleep = zzz<10 ? sheep++ : zzz++;
    System.out.print(sheep + "," + zzz); // 2,1
    ```
    - Since the left-hand `boolean` expression was
`true`, only `sheep` was incremented. Contrast 
the preceding example:
      - ```java
        int sheep = 1;
        int zzz = 1;
        int sleep = sheep>=10 ? sheep++ : zzz++;
        System.out.print(sheep + "," + zzz); // 1,2
        ```
      - The left-hand `boolean` expression evaluates to
`false`, only `zzz` is incremented.