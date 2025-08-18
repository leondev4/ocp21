---

markmap:
  initialExpandLevel: 1
---
# **Working with `switch`<br/> Expressions**
- **Returning Consistent Data Types**
  - When assigning a value as the result of a `switch` expression, 
  a branch can’t return a value with an unrelated type.
    - ```java
      int measurement = 10;
      int size = switch(measurement) {
        case 5 -> Integer.valueOf(1);
        case 10 -> (short)2;
        default -> 3;
        case 20 -> "4"; // DOES NOT COMPILE
        case 40 -> 5L;  // DOES NOT COMPILE
        case 50 -> null;// DOES NOT COMPILE
      };
      ```
      - The `switch` expression is being assigned to an `int` variable, so all of the values must 
      be consistent with `int`. The first `case` clause compiles without issue, as the `Integer` 
      value is unboxed to `int`.
      The second and third `case` clauses are fine, as they can be stored as an `int`. The 
      last three `case` expressions do not compile because each returns a type that is 
      incompatible with `int`.
- **Exhausting the `switch` Branches**
Unlike a `switch` statement, a `switch` 
expression must return a value.
  - ```java
    void identifyType(String type) {
      Integer reptile = switch (type) { // DOES NOT COMPILE
        case "Snake" -> 1;
        case "Turtle" -> 2;
      };
    }
    ```
    - What is the value of `reptile` if `type` is not equal to `Snake` or `Turtle`?
    Java decided this behavior would be unsupported and triggers a 
    compiler error if the `switch` expression is not exhaustive.
      - A `switch` is said to be exhaustive if it covers all possible values.
      **All `switch` expressions must be exhaustive, which means 
      they must handle all possible values**
        -  There are three ways to write 
          an exhaustive `switch`:
            - 1. Add a `default` clause.
            - 2. If the `switch` takes an enum, add a `case`
                clause for every possible enum value.
            - 3. Cover all possible types of the `switch`
                variable with pattern matching.
- In practice, the first solution is the one most often used.
You can try writing `case` clauses for all possible `int` values,
but we promise it doesn’t work! Even smaller types like `byte`
are not permitted by the compiler, despite there being only
`256` possible values.
  - The second solution applies only to `switch` expressions that
  take an enum. For example, consider the following:
    - ```java
      enum Season{SPRING, SUMMER, FALL, WINTER}

      String getWeatherMissingOne(Season value) {
        return switch (value) { // DOES NOT COMPILE
          case WINTER -> "Cold";
          case SPRING -> "Rainy";
          case SUMMER -> "Hot";
        };
      }
      ```
      - This code does not compile because `FALL` is not covered. The fix is 
      either to add a case for `FALL` or add a `default` clause (or both).
        - ```java
          String getWeatherCoveredAll(Season value) {
            return switch (value) {
              case WINTER -> "Cold";
              case SPRING -> "Rainy";
              case SUMMER -> "Hot";
              case FALL -> "Warm";
              default -> throw new
                RuntimeException("Unsupported Season");
              };
          }
          ```
        - Since all possible values of `Season` are covered, the 
        `default` branch is optional.
- **Using the `yield` Statement**
 A switch expression supports both `case` expressions and 
 `case` blocks, the latter of which is denoted with braces 
 (`{}`). The [Figure 3.6](https://1drv.ms/i/c/c83cfca51d5c2032/Ee-05I-cE-xKlc8aXJcUoP8B-pIJ8Dv2kUr0BAAHKdUvwg?e=KUeJJw) shows examples of both.
  - We said that a `switch` expression must return a value. But 
  how do you return a value from a `case` block?
Enter the `yield` statement, shown in Figure. It allows the
 `case` clause to return a value. For example, the following
uses a mix of `case` expressions and `case` blocks:
    - ```java
      int fish = 5;
      int length = 12;
      var name = switch (fish) {
        case 1 -> "Goldfish";
        case 2 -> { yield "Trout"; }
        case 3 -> {
          if (length> 10) yield "Blobfish";
          else yield "Green";
        }
        case 4 -> {
          throw new RuntimeException("Unsupported value");
        }
        default -> "Swordfish";
      };
      ```
      - Think of the `yield` keyword as a `return` statement within a
      `switch` expression. Because a `switch` expression must return
      a value, a `yield` is often required within a `case` block. The 
      one "exception" to this rule is if the code throws an exception, 
      as shown in the previous example.
      - **Watch Semicolons in `switch` Expressions**
        - When writing a `case` expression, a semicolon is required, but when writing a `case` 
        block, it is prohibited.
          ```java
          int fish = 1;
          var name = switch (fish) {
            case 1 -> "Goldfish" // DOES NOT COMPILE (missing semicolon)
            case 2 -> { yield "Trout"; }; // DOES NOT COMPILE (extra semicolon)
            default -> "Shark";
          } // DOES NOT COMPILE (missing semicolon)
          ```
  - Remember: <strong style="color:green">A `yield` is required within a case block
   unles an exception is thrown.</strong>