# **Handling a `null` Case**
- What if the `switch` variable is `null`?
  - ```java
    String fish = null;
    System.out.print(switch (fish){
        case "ClownFish" -> "Hello!";
        case "BlueTang" -> "Hello again!";
        default -> "Goodbye";
    });
    ```
    - This code compiles ( it technically is exhaustive )
    but throws a `NullPointerException`! 
    - New to Java 21, `switch` now supports `case null` 
    clause when working with object types
      - ```java
        String fish = null;
        System.out.print(switch (fish) {
          case "ClownFish" -> "Hello!";
          case "BlueTang" -> "Hello again!";
          case null -> "What type of fish are you?";
          default -> "Goodbye";
        });
        ```
      - Although the code looks like a `switch` expression, 
      it's a pattern matching. Look carefully at the order of 
      `case null` and `default`.
- **Case `null` Is Considered Pattern 
  Matching**
  - Any guess as to why the following code 
  snippet does not compile?
    - ```java
      String fish = null;
      switch (fish) { // DOES NOT COMPILE
        case "ClownFish": System.out.print("Hello!");
        case "BlueTang":  System.out.print("Hello again!");
        case null:        System.out.print("null");
      }
      ```
      - Anytime `case null` is used within a `switch`, then the `switch` 
      statement is considered to use pattern matching.
      - That means the `switch` statement _must be exhaustive_. 
      Adding a `default` branch allows the code to compile.
- Since using `case null` implies pattern matching, the ordering 
of branches matters anytime `case null` is used. **While
`case null` can appear almost anywhere in `switch`, it 
cannot be used after a `default` statement.**
  - ```java
    System.out.print(switch (fish) {
      case String s when "ClownFish".equals(s) -> "Hello!";
      case null -> "No good";
      case String s when "BlueTang".equals(s) -> "Hello again!";
      default -> "Goodbye";
    });
    ```
    - It compiles
  - ```java
    System.out.print(switch (fish) {
      case String s when "ClownFish".equals(s) -> "Hello!";
      case String s when "BlueTang".equals(s) -> "Hello again!";
      default -> "Goodbye";  
      case null -> "No good";  // DOES NOT COMPILE
    });
    ```
    - It does not compile