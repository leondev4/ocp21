# **Selecting the `switch` Variable**
- A `switch` has a target variable that is not evaluated until runtime. 
The following is a list of all data types supported by `switch`:
  - 1.- `int` and `Integer`
    2.- `byte` and `Byte`
    3.- `short` and `Short`
    4.- `char` and `Character`
    5.- `String`
    6.- enum values
    7.- All object types (when used with pattern matching)
    8.- `var` (if the type resolves to one of the preceding types)
    - **Notice that `boolean`, `long`, `float`, and `double` are not 
    supported in `switch` statements and `switch` expressions.**
    - For this chapter, you just need to know that an enumeration, or
    _enum_, is a type in Java that represents a fixed set of constants.
      - ```java
        enum Season { SPRING, SUMMER, FALL, WINTER }
        ```
        - Think of an enum as a class type with predefined
        object values known at compile time.
          - For now, you just need to think of them as a list of values.
      - ```java
        enum DayOfWeek {
          SUNDAY, MONDAY, TUESDAY, WEDNESDAY, 
          THURSDAY, FRIDAY, SATURDAY
         }
        ```
        - **Using Enum Values with `switch`**
          When the `switch` variable is an enum type, then
          the case clauses must be the enum values.
          - ```java 
            enum Season { SPRING, SUMMER, FALL, WINTER }
              boolean shouldGetACoat(Season s) {
                return switch (s) {
                  case SPRING -> false;
                  case Season.SUMMER -> false;
                  case FALL -> true;
                  case Season.WINTER -> true;
                };
              }
              ```
          - Note that there is no `default`. For an enum value, you can 
            specify just the value, as shown with `SPRING` and `FALL`. 
            Starting with Java 21, you can optionally specify the name 
            with the value, such as `Season.SPRING` and `Season.FALL`.
    - Starting with Java 21, `switch` statements and expressions now support 
    pattern matching, which means any object type can now be used as a 
    `switch` variable, provided the pattern matching is used.
- **Determining Acceptable Case Values**
The values in each `case` clause must be compile-time constant
values.**You can use only literals, enum constants, or `final` 
constant variables.**
  - By `final` constant, we mean that the variable must be
marked with the `final` modifier and initialized with a literal
value in the same expression in which it is declared. For
example, **you can’t have a `case` clause value that 
requires executing a method at runtime, even if that 
method always returns the same value.**
    - See if you can figure out why only the
first and last case clauses compile:
      - ```java
        final int getCookies() { return 4; }
        void feedAnimals() {
          final int bananas = 1;
          int apples = 2;
          int numberOfAnimals = 3;
          final int cookies = getCookies();
          switch (numberOfAnimals) {
            case bananas:
            case apples: // DOES NOT COMPILE
            case getCookies(): // DOES NOT COMPILE
            case cookies: // DOES NOT COMPILE
            case 3 * 5:
        } }
        ```
        - In the `case` clause, you cannot use methods or variables initialized with methods.
        - The `bananas` variable is marked `final`, and its value is known at compile time, so 
        the first `case` clause is valid. The `apples` variable in the second `case` clause is 
        not marked `final`, so it is not permitted. The next two `case` clauses, with values 
        `getCookies()` and `cookies`, do not compile because methods are not evaluated 
        until runtime, so they cannot be used as the value of a `case` clause, even if one 
        of the values is stored in a `final` variable. The last `case` clause, with value `3*5`, 
        does compile, as expressions are allowed as `case` values, provided the value 
        can be resolved at compile time.
          - The data type for `case` clauses must match the
data type of the `switch` variable.
          - ```java
            String cleanFishTank(int dirty) {
              return switch (dirty) {
                case "Very" -> "1 hour"; // DOES NOT COMPILE
                default     -> "45 minute";
              };
            }
            ```
          - The `switch` variable is of type `int`, while the case clause is of
          type `String`.
- **Working with `switch` Statements**
A `break` statement terminates the `switch` statement and returns 
flow control to the enclosing process. Put simply, it ends the 
`switch` statement immediately.
  - While `break` statements are optional, they tend to be used
frequently in `switch` statements. Without `break` statements,
the code continues to execute the next branch it finds, in order.
    - What do you think the following prints when
  `printSeasonForMonth(2)` is called?
      ```java 
      void printSeasonForMonth(int month) {
        switch(month){
          case 1, 2, 3: System.out.print("Winter-");
          case 4, 5, 6: System.out.print("Spring-");
          default:      System.out.print("Unknown-");
          case 7, 8, 9: System.out.print("Summer-");
          case 10, 11, 12: System.out.print("Fall-");
        } }
      ```
      - It prints everything!
      `Winter-Spring-Unknown-Summer-Fall-`
      - It matches the first `case` clause and executes all of the
  branches in the order they are found, including the `default`
  clause. Remember when working with `switch` statements,
  _the order of the branches is important!_
        - We can fix this to just print `Winter-` with the same input, by adding `break` statements.
          ```java
          void printSeasonForMonth(int month) {
            switch (month) {
              case 1,2,3:
                System.out.print("Winter-"); break;
              case 4,5,6:
                System.out.print("Spring-"); break;
              default:
                System.out.print("Unknown-"); break;
              case 7,8,9:
                System.out.print("Summer-"); break;**
              case 10,11,12: System.out.print("Fall-"); break;
          } }
          ```
          - The last `case` clause does not actually require a `break`, 
          as the `switch` statement is over,
          - Contrast this with a `switch` expression that matches only 
          a single branch at runtime and therefore does not require 
          `break` statements.
            ```java
            void printSeasonForMonth(int month) {
              String value = switch (month) {
                case 1, 2, 3 -> "Winter-";
                case 4, 5, 6 -> "Spring-";
                default -> "Unknown-";
                case 7, 8, 9 -> "Summer-";
                case 10, 11, 12 -> "Fall-";
              };
              System.out.print(value);
            }
            ```
- **When Is a `switch` Expression Not a switch Expression?**
A `switch` expression always returns a value, regardless of the 
syntax used.
  - ```java
    void printWeather(int rain) {
      switch (rain) {
        case 0 -> System.out.print("Dry"); 
        case 1 -> System.out.print("Wet"); 
        case 2 -> System.out.print("Storm"); 
      }
    }
    ```
    - Since the return type of `System.out.print()` is `void`, this
statement does not return a value. This is actually a
 `switch` statement that uses the arrow operator syntax. 
 Since it doesn’t return a value, it is not a `switch`
expression. Most of the time `switch` expression needs
 `default` statement.