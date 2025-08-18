---

markmap:
  initialExpandLevel: 1
---
# **Understanding the Class Structure**
- In Java programs, classes are the basic building blocks. When defining a _class_,
you describe all the parts and characteristics of one of those building blocks.
  - An _object_ is a runtime instance of a class in memory. An object is often referred to
  as an _instance_ since it represents a single representation of the class. All the various
  objects of all the different classes represent the state of your program. A _reference_ is
  a variable that points to an object.
- **Fields and Methods**
  - Java classes have two primary elements: **methods**, often called functions or procedures in
   other languages, and**fields**, more generally known as variables. **Together these are called
    the members of the class**. Variables hold the state of the program, and methods operate
     on that state. If the change is important to remember, a variable stores that change.
- The simplest Java class
  ```java
  1: public class Animal{
  2: }
  ```
  - Java calls a word with special meaning a _keyword_, which we’ve marked bold. We
   often bold parts of code snippets to call attention to them. `public` keyword, allows
    other classes to use it. The `class` keyword indicates you’re defining a class. `Animal` 
    gives the name of the class.
    - You can add your first field.
      ```java
      1: public class Animal{
      2:   String name;
      3: }
      ```
      - On line 2, we define a variable named `name`. We also declare the type
    of that variable to be `String`. A `String` is a value that we can put text
    into, such as `"this is a string"`.
- We can add methods.
  ```java
  1: public class Animal{
  2:   String name;
  3:   public String getName(){
  4:     return name;
  5:   }
  6:   public void setName(String newName){
  7:     name = newName;
  8:   }
  9: }
  ```
  - On lines 3–5, we define a method. A method is an operation that can
be called. Again, `public` is used to signify that this method may be
called from other classes. Next comes the return type—in this case,
the method returns a `String`. On lines 6–8 is another method. This
one has a special return type called `void`. The `void` keyword means
that no value at all is returned. This method requires that information
 be supplied to it from the calling method; this information is called a
  _parameter_. The `setName()` method has one parameter named
   `newName`, and it is of type `String`. This means the caller should pass 
   in one `String` parameter and expect nothing to be returned.
    - **Method signature.**
    The **method name and parameter types** 
    are called the method signature.
      - In this example, can you identify the method
        name and parameters?
        ```java
        public int numberVisitors(int month){
          return 10;
        }
        ```
          - The method name is `numberVisitors`. There’s one
          parameter named `month`, which is of type `int`,
          which is a numeric type. Therefore, the **method
          signature is `numberVisitors(int)`**.
- **Classes and Source Files**
A _top-level_ type is a data structure that can be
defined independently within a source file. 
  - A top-level class is often `public`, which means any code
   can call it. Java does not require that the type be `public`. 
   For example, this class is just fine:
    ```java
    1: class Animal{
    2:   String name;
    3: }
    ```
    - You can even **put two types in the same file**. 
    When you do so, **at most one of the top-level
     types in the file is allowed to be `public`.** That
      means a file containing the following is also fine:
      ```java
      1: public class Animal{
      2:    private String name;
      3: }
      4: class Animal2{}
      ```
      - **If you do have a `public` type, it needs to match the 
      filename.** The declaration `public class Animal2`
       would not compile in a file named `Animal.java`.