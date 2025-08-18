---

markmap:
  initialExpandLevel: 1
---
# **Equality Operators**
- The equals operator (`==`) and not equals operator (`!=`) 
compare two operands and return a `boolean` value 
determining whether the expressions or values are equal 
or not equal, respectively.
  - Equality `a == 10`
    - Returns `true` if the two values 
    represent the same value
      - Returns `true` if the two values 
      reference the same object
  - Inequality `b != 3.14`
    - Returns `true` if the two values 
represent different values
      - Returns `true` if the two values do not 
reference the same object
- The equality operator can be applied to numeric values,
`boolean` values, and objects (including `String` and `null`). 
You cannot mix these types.
  - ```java
    boolean monkey = true == 3; // DOES NOT COMPILE
    boolean ape = false != "Grape"; // DOES NOT COMPILE
    boolean gorilla = 10.2 == "Koko"; // DOES NOT COMPILE
    ```
    - Pay close attention to the data types when you see an
equality operator on the exam. As mentioned in the
previous section, the exam creators also have a habit of
mixing assignment operators and equality operators.
      -   ```java
          boolean bear = false;
          boolean polar = (bear = true);
          System.out.println(polar); // true
          ```
          - At first glance, you might think the output should be `false`, and if 
          the expression were (`bear == true`), then you would be correct. 
          In this example, though, the expression is assigning the value of 
          `true` to `bear`, and as you saw in the section on assignment 
          operators, the assignment itself has the value of the assignment. 
          Therefore, `polar` is also assigned a value of `true`, and the output 
          is `true`.
- **For object comparison, the equality operator is applied
to the references to the objects, not the objects they point
to. Two references are equal if and only if they point to the
same object or both point to `null`.** Let’s take a look at some 
examples:
  - ```java
    var monday = new File("schedule.txt");
    var tuesday = new File("schedule.txt");
    var wednesday = tuesday;
    System.out.println(monday == tuesday); // false
    System.out.println(tuesday == wednesday); //true
    ```
    - Even though all of the variables point to the same file
information, only two references, `tuesday` and 
`wednesday`, are equal in terms of `==` since they 
point to the same object.
    - ```java
      boolean result = null == null; // true
      ```