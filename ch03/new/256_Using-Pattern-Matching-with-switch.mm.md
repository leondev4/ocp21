---

markmap:
  initialExpandLevel: 1
---

# **Using Pattern Matching<br/>with `switch`**
- To use pattern matching with a `switch`, first start with an 
object reference variable. Any object reference type is 
permitted, provided the `switch` makes use of pattern
matching. Next, in each case clause, define a type and
pattern matching variable.
  - ```java
    void printDetails(Number height) {
      String message = switch (height) {
        case Integer i -> "Rounded: " + i;
        case Double d -> "Precise: " + d;
        case Number n-> "Unknown: " + n;
      }; // watch semicolon at the end
      System.out.print(message);
    }
    ```
    - In this example, we output different values depending on
the type of the `switch` variable. The same rules about local
variables and flow scoping that we learned about earlier 
with pattern matching apply. For instance, _the pattern 
matching variable exists only within the `case` branch for
which it is defined. This allows us to reuse the same 
name for two `case` branches._
      - [Figure 3.7](https://1drv.ms/i/c/c83cfca51d5c2032/Ed24JgQv7nxJma5-xkoO7wkBKydnKGPuWYs-7SjEJ1RxAw?e=y5pZZd) shows the structure of pattern matching with a 
      `switch` expression. **It can also be used with a `switch` 
      statement that does not return a value.**

        - From Figure 3.7, you might have noticed it includes a guard clause, 
        which is an optional conditional clause that can be added to a `case` 
        branch. This is similar to what we saw in  pattern matching if statement. 
        The only difference is that with `switch`, the `when` keyword is required 
        between the variable and the expression. The `when` statement comes 
        before the general type.
      - Remembre: **Type matches `switch` variable reference 
      type so `default` clause is not needed.**
- Example:
  ```java
  String getTrainer(Number height) {
    return switch (height) {
      case Integer i when i > 10 ->"Joseph";
      case Integer i -> "Daniel";
      case Double num when num <= 15.5 ->"Peter";
      case Double num -> "Kelly";
      case Number num -> "Ralph";
    };
  }
  ```
  - Note that what is in the `when` statement always comes first.
`Joseph` works with the animal if the `height` is an `Integer` 
greater than `10`. `Daniel` is then selected for all other `Integer` 
values. Likewise, `Peter` handles all `Double` measurements less 
than or equal to `15.5`, while `Kelly` handles the remaining
`Double` values. Finally, `Ralph` handles all animals that don’t 
meet one of the previous requirements, such as if `Short` or
 `Long` was used.
    - One advantage of guards is that now `switch` can do
    something it’s never done before: it can handle ranges.
    Previously, if you wanted to support a range of values
    with a `switch`, you had to list all the possible `case`
    values. With the `when` clause, you can support range 
    matches.
      - **Applying Acceptable Types**
      One of the simplest rules when working with `switch` and
      pattern matching is that the type must be the same type
      as the `switch` variable or a subtype.
      - ```java
        Number fish = 10;
        String name = switch (fish) {
          case Integer freshWater -> "Bass";
          case Number saltWater -> "ClownFish";
          case String s -> "Shark"; // DOES NOT COMPILE
        };
        ```
        - The compiler is smart enough to know a `Number` can’t be 
        cast as a `String`, resulting in this code not compiling.
- **Ordering switch Branches**
The order of `case` and `default` clauses for `switch` statement matters, 
because more than one branch might be reached  during execution. 
For `switch` expressions that don’t use pattern matching, ordering 
isn’t important, as only one branch can be reached.
  - When working with pattern matching, the order
    matters regardless of the type of `switch`!
    - ```java
      void printDetails(Number height) {
        String message = switch (height) {
            case Number n -> "Unknown: " + n;
            case Integer i -> "Rounded: " + i;
            case Double d -> "Precise: " + d;
          };
          System.out.print(message);
      }
      ```
      - It is impossible for a process to reach the second and third `case`
      clauses. This is also referred to as unreachable code (when the 
      compiler detects it, a compilation error occurs).
        - Ordering branches is also important if a `when` clause is used.
        - ```java
          String getTrainer(Number animal) {
            return switch (animal) {
              case Integer i -> "Daniel";
              case Integer i when i > 10 -> "Joseph"; // DOES NOT COMPILE
              …
             };
          }
          ```
        - It goes from the most specific to the most general, similar to multiple catch blocks.
      - The last two `case` statements are unreachable. Go from 
      the most specific type to the most general.
- **Exhaustive `switch` Statements**
Up until now, only `switch` expressions were required to be
exhaustive. When using pattern matching, `switch` statements
must be exhaustive too. As before, you can address this with 
a `default` clause, as this will handle all values that don’t match 
a `case` clause. There is another option, though, that applies 
only when working with pattern matching.
  - You may have noticed that **we didn’t use any `default` clauses in 
  any of our previous pattern matching examples. That’s because 
  we defined our last `case` clause with a pattern matching variable 
  type that is the same as the switch variable reference type.**
    - For example, **if the variable reference type of the switch expression 
    is type `Integer` or `String`, then you just need to make sure the last 
    `case` clause is of type `Integer` or `String`**, respectively.
      - ```java
        Number zooPatrons = Integer.valueOf(1_000);
        switch (zooPatrons) {
          case Integer count-> System.out.print(count);
        }
        ```
        - It doesn’t compile! Despite the `zooPatrons` object actually being of 
        type `Integer`, the `switch` reference variable is of type `Number`.
-  There are a few ways that we can fix the last example. First, we
can change the reference type of `zooPatrons` to be `Integer`
    - ```java
      Integer zooPatrons = Integer.valueOf(1_000);
      switch (zooPatrons) {
        case Integer count-> System.out.print(count);
      }
      ```
      - We can also add a `case` clause at the end for `Number`.
        - ```java
          Number zooPatrons = Integer.valueOf(1_000);
          switch (zooPatrons) {
            case Integer count -> System.out.print(count);
            case Number count -> System.out.print("Number");
          }
          ```
          - We can always add a `default` clause to a `switch` that covers 
          everything.  What if you combine different solutions?
            - ```java
              Number zooPatrons = Integer.valueOf(1_000);
              switch (zooPatrons) {
                case Integer count -> System.out.print(count);
                case Number count -> System.out.print("Number");
                default -> System.out.print("Default");
              }
              ```
            - The code does not compile, regardless of how you order the branches. 
            The compiler is smart enough to realize the last two statements are 
            redundant