---

markmap:
  initialExpandLevel: 1
---
# **Lambda Syntax**
- Lambdas work with interfaces that have exactly one abstract method.
- Functional interface
  - ```java
    public interface CheckTrait{
      boolean test(Animal a); //SAM
    }
    ```
- **Only meets the SAM rule of a functional interface**
  - ```java
    print(animals,a-­>a.canHop());
    ```
    The `print()` method expects a `CheckTrait` as the second parameter:
    ```java
    static void print(List<Animal> animals,CheckTrait checker) { ... }
    ```
    - Since we are passing a lambda, Java tries to map our lambda to 
    the abstract method declaration in the `CheckTrait` interface:
**`boolean test(Animal a);`**
Since that interface’s method takes an `Animal`, the lambda 
parameter has to be an `Animal`. And since that interface’s 
method returns a `boolean`; the lambda returns a `boolean`.
- Lambda syntax omitting optional parts
  - ```java
    a -> a.canHop()
    ```
    - **1.** A single parameter specified with the name `a` 
      **2.** The arrow operator (`->`) to separate the parameter and body 
      **3.** A body that calls a single method and returns the result of that method
- Lambda syntax including optional parts
  - ```java
    (Animal a) -> { return a.canHop(); }
    ```
    - **1.** A single parameter specified with the name `a` and stating that the type is `Animal` and parentesis 
    **2.** The arrow operator (`->`) to separate the parameter and body 
    **3.** A body that has one or more lines of code, including braces, a semicolon and a `return` statement
- You can mix syntax
  - ```java
    a -> {return a.canHop();}
    ```
    - The parentheses around the lambda parameters can be omitted only
     if there is a single parameter and its type is not explicitly stated.
  - ```java
    (Animal a)->a.canHop()
    ```
    - You can omit braces when we have only a single statement. We did this with if statements and
    loops already. Java allows you to omit a `return` statement and semicolon (`;`) when no braces
    are used. This special shortcut doesn’t work when you have two or more statements.
- You cannot assign a lambda using `var`
  - ```java
    var lambda = a -> a.canHop(); // Error: cannot use var with lambdas
    ```
- Valid lambdas that return a `boolean`
  - ```java
    () -­> true                                   //0 parameters
    x -­> x.startsWith("test")                    //1 parameter
    (String x) -­> x.startsWith("test")           //1 parameter
    (x, y) -­> { return x.startsWith("test"); }   //2 parameters
    (String x, String y) -­> x.startsWith("test") //2 parameters
    ```
- Invalid lambdas that should return a `boolean`
  - ```java
    x, y -­> x.startsWith("fish")            //Missing parentheses on left
    x -­> { x.startsWith("camel"); }         //Missing return on right
    x -­> { return x.startsWith("giraffe") } //Missing semicolon inside braces
    String x -­> x.endsWith("eagle")         //Missing parentheses on left
    ```