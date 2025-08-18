---

markmap:
  initialExpandLevel: 1
---
# **Conditional operators**
- The conditional operators, often called short-circuit operators, are nearly identical to the logical operators, 
`&` and `|`, except that the right side of the expression may never be evaluated if the final result can be 
determined by the left side of the expression.
  - Conditional AND `a && b`
    - The value is `true` only if both values are `true`. If the left side is `false`,
    then the right side will not be evaluated.
  - Conditional OR `c || d`
    - The value is `true` if at least one of the values is `true`. If the left side is `true`, 
    then the right side will not be evaluated.
      - ```java
        int hour = 10;
        boolean zooOpen=true||(hour < 4);
        System.out.println(zooOpen); // true
        ```
        - Since we know the left side is `true`, there’s no need to evaluate the right side. 
        `hour` could have been `-10` or `892`; the output would have been the same. 
- **Avoiding a `NullPointerException`**
A more common example of where conditional operators are used is checking for `null` 
objects before performing an operation. In the following example, if `duck` is `null`, the
program will throw a `NullPointerException` at runtime:
  - ```java
    if(duck != null & duck.getAge() < 5) { // Could throw a NullPointerException
      // Do something
    }
    ```
  - The issue is that the logical AND (`&`) operator evaluates both sides of the expression.
    -  An easy-to-read solution is to use the
conditional AND operator (`&&`)
        - ```java
            if(duck != null && duck.getAge()<5){
              // Do something
            }
          ```
          - If `duck` is `null`, the conditional prevents a `NullPointerException` from ever being thrown, 
          since the evaluation of `duck.getAge() < 5` is never reached.
- **Checking for Unperformed Side Effects**
  - Be wary of short-circuit behavior on the exam, as questions
are known to alter a variable on the right side of the
expression that may never be reached. This is referred to
as an _unperformed side effect_.
    - ```java
      int rabbit = 6;
      boolean bunny = (rabbit>= 6)||(++rabbit <= 7);
      System.out.println(rabbit);
      ```
      - Because `rabbit >= 6` is `true`, the increment operator on the right 
      side of the expression is never evaluated, so the output is `6`.