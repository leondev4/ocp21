---

markmap:
  initialExpandLevel: 1
---
# **Introducing `switch`**
- A `switch` is a complex decision-making structure in which a single 
value is evaluated and flow is redirected to one or more branches.
  - There are two flavors: a `switch` statement and a `switch` expression. 
  The primary difference between the two is that a `switch` expression 
  must return a value, while a `switch` statement does not.
    - Example of `switch` statement:
      - ```java
        String getAnimalBetter(int type){
          String animal;
            switch(type){
              case 0:
                animal = "Lion";
                break;
              case 1:
                animal = "Elephant";
                break;
              case 2,3: //separated by commas, not colons
                animal = "Alligator";
                break;
              default:
                animal = "Unknown";
            }
            return animal;
        }
        ```
        - The `break` in `default` is not required since it is at the end.
        - It’s really long. We’re assigning `animal` tree times, and what’s with all 
        the `break` statements?. Let’s try using a `switch` expression instead.
        - ```java
          String getAnimalBest(int type) {
            return switch(type){
              case 0->"Lion";
              case 1->"Elephant";
              case 2, 3->"Alligator";
              case 4->"Crane";
              default->"Unknown";
            }; //note ; at the end
          }
          ```
          - That is a lot shorter and easier to read! In Java, `switch` expressions 
          were introduced more recently than `switch` statements, resulting in 
          a greater emphasis on reducing boilerplate.
- **Defining a `switch`**
Both types begin with a `switch` keyword and a variable 
in parentheses, followed by a pair of braces. **In switch
expression semicolon is required at the end.**
  - ```java
    String name = "123";
    // Switch statement
    switch(name){             
      case "Sancha": System.out.print(1);       break;
      case "Jacob","Jake": System.out.print(2); break;
      default: System.out.print(999);           break;
    }
    ```
    - Both types of `switch` support zero or more `case` clauses. Each 
    `case` clause includes a set of matching values split up by commas 
    (`,`). It is then followed by a separator, which can be a colon (`:`) or 
    the arrow operator (`->`). Finally, each clause then defines an 
    expression, or code block with braces (`{}`), for what to execute when 
    there’s a match. Note that only `switch` statement has `break`.
      - **Using the Arrow Operator with switch Statements**
        While switch statements support both colons and arrow operators, 
        you’re likely to see them used with colons more often in practice. 
        **If you do use the arrow operator, then you must use it for all 
        clauses**.
          - The following `switch` statement does not compile:
          - ```java
            switch (type) {
              case 0 : System.out.print("Lion");
              case 1 -> System.out.print("Elephant");
            }
            ```
            - This would compile if both clauses used the same
separator.
  - ```java
    var n = switch(name){ // Switch expression
        case "Sancha"        -> 1;
        case "Jacob", "Jake" -> 2;
        default              -> 999;
      }; // watch ";" at the end
    ```
    - Both `switch` types support an optional `default` clause. 
    With `switch` expressions, a `default` clause is often
    required, as the expression must return a value. 
      - A `switch` statement is not required to contain any `case`
      clauses. This is perfectly valid:
        ```java
        switch (month) {}
        ```
- [Figure 3.4](https://1drv.ms/i/c/c83cfca51d5c2032/EeGRBr8BQzFJigkw0wLA4HoBteZ5y-aGmY-kS4X4hRUjMQ?e=gNFavS) shows the structure of a `switch` statement. While 
`switch` statements can use the arrow operator, they are more
 commonly written with colons.
  - [Figure 3.5](https://1drv.ms/i/c/c83cfca51d5c2032/EY_yBCylqEdBpmVoEeZwQusBaSQ1AE6J1wHU0E8NC6paYQ?e=yVyMd2) shows the structure of a `switch` expression, 
  which returns a value that is assigned to a variable. 
    - Unlike a `switch` statement, a `switch` expression often requires a semicolon (`;`) 
    after it, such as when it is used with the assignment operator (`=`) or a `return` 
    statement.
      - A `switch` expression also requires a semicolon (`;`) after each 
      `case` expression that doesn’t use a block. For  example, how 
      many semicolons are missing in the following?
        - ```java
          var result = switch (bear) {
            case 30 -> "Grizzly"
            default -> "Panda"
          }
          ```
          - The answer is three. Each `case` or `default`  expression 
          requires a semicolon as well as the assignment itself. 
          The following fixes the code:
            - ```java
              var result = switch (bear) {
                case 30 -> "Grizzly";
                default -> "Panda";
              };
              ```
- See if you can figure out which of these
compiles:
  - ```java
    int food = 5, month = 4, weather = 2, 
        day = 0, time = 4;

    String meal = switch food { // #1
      case 1 -> "Dessert"
      default -> "Porridge"
    };

    switch (month) // #2
      case 4: System.out.print("January");

    switch (weather) { // #3  
      case 2: System.out.print("Rainy");
      case 5: {
        System.out.print("Sunny");
      }
    }
    ```
    - The **first statement** does not compile because the `switch` expression 
    is missing parentheses around the `switch` variable, as well as semicolons 
    after the `case` and `default` clauses. The **second statement** does not 
    compile because it is missing braces around the `switch` body. The **third 
    statement** does compile. Notice that a `case` clause can use an expression 
    or a code block with braces.
      - ```java
        switch (day) { // #4
          case 1: 13: System.out.print("January");
          default
          System.out.print("July");
        }

        String description = switch (time){ // #5
          case 10 -> "Morning";
          default -> "Late";
        }
        ```
        - The **fourth statement** does not compile for two reasons. 
        The `case` clause should use a comma (`,`) to separate two 
        values, not a colon (`:`). It’s also missing a colon after the 
        `default` clause. The **last statement** does not compile because 
        the assignment operator (`=`) is missing a semicolon  (`;`) at 
        the end.