---

markmap:
  initialExpandLevel: 1
---

# **Passing Constructor and<br/>Method Parameters**
- Variables passed to a constructor or method are called constructor
parameters or method parameters.
  - These parameters are like local variables that have been preinitialized. 
- In this method, `check` is a method parameter.
  ```java
  public void findAnswer(boolean check){}
  ```
  - Take a look at the following method `checkAnswer()` in the same class:
    ```java
    public void checkAnswer() {
      boolean value;
      findAnswer(value); // DOES NOT COMPILE
    }
    ```
    - The call to `findAnswer()` does not compile because it tries to use a
    variable that is not initialized. While the caller of a method
    `checkAnswer()` needs to be concerned about the variable being
    initialized, once inside the method `findAnswer()`, we can assume the
    local variable has been initialized to some value.
- **Defining Instance and Class Variables**
  - Variables that are not local variables are defined either as instance
  variables or as class variables. An _instance variable_, often called a
  field, is a value defined within a specific instance of an object. Let’s
  say we have a `Person` class with an instance variable `name` of type
  `String`. Each instance of the class would have its own value for `name`, 
  such as `Elysia` or `Toby`. Two instances could have the same value for 
  `name`, but changing the value for one does not modify the other.
    - ```java
      public class Person {
        String name;
      }
      ```
  - A _class variable_ is one that is defined on the class level and shared 
  among all instances of the class. It can even be publicly accessible
    to classes outside the class and doesn’t require an instance to use. 
  You can tell a variable is a class variable because it has the keyword 
  `static` before it. 
    - ```java
      public class Person {
        static int instances;
      }
      ```
  - Instance and class variables do not require you to initialize them.
  As soon as you declare these variables, they are given a default
  value.
    - `null` for object
    - zero for `byte`, `short` and `int`
    - `0L` for `long`, if you print it get `0`
    - `0.0f` for `float`, if you print it get `0.0`
    - `0.0` for `double`.
    - `'\u0000'` for `char`
  - ```java
    public class Person {
    int v; long l; float f; double d;
    public static void main(String[] args) {
      var p = new Person();
      System.out.println(p.v);
      System.out.println(p.l);
      System.out.println(p.f);
      System.out.println(p.d);
      }
    }
    ```
    - It prints:
      ```java
      0
      0
      0.0
      0.0
      ```