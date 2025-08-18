---

markmap:
  initialExpandLevel: 1
---
# **Working with Variables<br/> in Lambdas**
- Parameter list
  - The type of parameters is optional. Additionally, `var` 
  can be used in place of the specific type. That means 
  that all three of these statements are interchangeable
    - ```
      Predicate<String> p = x -> true;
      ```
      - You can add the `final` modifier or an annotation
        - ```java
          public void counts(List<Integer> list) {
            list.sort((final var x, @Deprecated var y) -> x.compareTo(y));
          }
          ```
    - ```
      Predicate<String> p= (var x) -> true;
      ```
      - A lambda infers the types from 
      the surrounding context.
        - Can you figure out the type of `x`?
          ```java 
          public void whatAmI() {
            consume((var x) -­> System.out.print(x), 123);
          }
          public void consume(Consumer<Integer> c, int num) {
            c.accept(num);
          }
          ```
          - `x` is `Integer`
            The `whatAmI()` method creates a lambda to be passed to the
             `consume()` method. Since the `consume()` method expects an
              `Integer` as the generic, we know that is what the inferred type 
              of `x` will be.
        - What do you think the type of `x` is here?
          ```java
          public void counts(List<Integer> list) {
            list.sort((var x, var y) -> x.compareTo(y));
          }
          ```
          - `x` is `Integer`.
Since we are sorting a list, we can use the type of the 
list to determine the type of the lambda parameter.
    - ```
      Predicate<String> p= (String x) -­>true;
      ```
  - You have three formats for specifying parameter types within a
   lambda: without types, with types, and with `var`. The compiler 
   requires all parameters in the lambda to use the same format.
    - ```java
      5: (var x, y) -­> "Hello" // DOES NOT COMPILE
      6: (var x, Integer y) -­> true // DOES NOT COMPILE
      7: (String x, var y, Integer z) -­> true // DOES NOT COMPILE
      8: (Integer x, y) -­> "goodbye" // DOES NOT COMPILE
      ```

      Lines 5 needs to remove `var` from `x` or add it to `y`. Next, lines 6 and 7 
      need to use the `type` or `var` consistently. Finally, line 8 needs to remove 
      `Integer` from `x` or add a type to `y`.
- Local variables declared <br/> inside the lambda body
  - You can define a block. That block can have anything that is valid 
  in a normal Java block, including local variable declarations.
    - ```java
      (a, b) -­> { int c = 0; return 5; }
      ``` 
      - It creates a local variable named `c` that is scoped to the lambda block.
    - ```
      (a, b) -­> { int a=0; return 5; } // DOES NOT COMPILE
      ```
      - Java doesn’t let you create a local variable with the 
      same name as one already declared in that scope.
    -  How many syntax errors do you see in this method?
        ```java
        11: public void variables(int a){
        12:    int b = 1;
        13:    Predicate<Integer> p1 = a->{
        14:      int b = 0;
        15:      int c = 0;
        16:      return b == c;}
        17: }
        ```
        - There are three syntax errors. The first is on line 13. The variable `a` 
        was already used in this scope as a method parameter, so it cannot
         be reused. The next syntax error comes on line 14, where the code 
         attempts to redeclare local variable `b`. The third syntax error is that
          the variable `p1` is missing a semicolon at the end on line 16.
- Variables referenced from
the lambda body
  - Lambda can access an instance variable, method parameter, or local variable under certain 
  conditions. Instance variables and class variables are always allowed. **The only thing 
  lambdas cannot access are variables that are not `final` or effectively final.**
    - ```
      public class Crow {
        private String color;
        private static int age;
        public void caw(String name) {
          String volume = "loudly";
          Consumer<String> consumer = s -­>
            System.out.println(name + " says " + volume + " that she is " + 
                  color+" age "+ age+" " + s);
        }
      }
      ```
      - Instance variable: `color`
        Class variable: `age`
        Lambda parameter:`s`
        Local variable: `volume`
        Method parameter: `name`
    - ```java
      2: public class Crow{
      3:   private String color;
      4:   public void caw(String name){
      5:     String volume = "loudly";
      6:     name = "Caty";
      7:     color = "black";
      8:`
      9:     Consumer<String> consumer = s -­>
      10:       System.out.println(name + " says "  // DOES NOT COMPILE
      11:          + volume + " that she is " + color);  // DOES NOT COMPILE
      12:    volume = "softly";
      13:   }
      14: }
      ```
      - If the variables `name` and `volume` were final, the compilation
       errors would be on lines 6 and 12; in the assignments
       - The method parameter `name` is not effectively final because it is
      set on line 6 (lambda is not allowed to use the variable, line 10).
      - The variable `volume` is not effectively final since it is updated
      on line 12 (lambda is not allowed to use the variable, line 11).
  - Always allowed
    - Instance variable
    - Class or Static variable
    - Lambda parameter
  - Allowed if are `final` or
   effectively final
    - Local variable
    - Method parameter