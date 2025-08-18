---

markmap:
  initialExpandLevel: 1
---
# **Writing a `main()` Method**
- A Java program begins execution with its `main()` method
  - The `main()` method is often called an entry point into the program, because it is
   the starting point that the JVM looks for when it begins running a new program.
- Example:
  ```java
  1: public class Zoo{
  2:   public static void main(String[] args){
  3:     System.out.println("Hello World");
  4:   }
  5: }
  ```
  - The `main()` method lets the JVM call our code
  - This code prints `Hello World`. To compile and execute this code,
   type it into a file called `Zoo.java` and execute the following:
    ```java
    javac Zoo.java
    java Zoo
    ```
    - To compile Java code with the `javac` command, **the file must 
    have the extension `.java`. The name of the file must match
     the name of the `public` class.** The result is a file of bytecode 
     with the same name but with a `.class` filename extension.
     - We must omit the `.class` extension to run `Zoo.class`.
     - To keep things simple for now, we follow this subset of the rules:
        -  1.- Each file can contain only one `public` class.
          2.- The filename must match the class name, including case, and
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;have a `.java` extension.
          3.- If the Java class is an entry point for the program, it must
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;contain a valid `main()` method.
  - **`public`**:  means full access from anywhere in the program.
    - **`static`**: binds a method to its class so it can be called by just
    the class name, as in, for example, `Zoo.main()`. Java doesn’t 
    need to create an object to call the `main()` method
      - **`void`**:  represents the return type. A method that returns
      no data returns control to the caller silently. 
  - **`main()` method’s parameter list**, represented as an array
  of `java.lang.String` objects. You can use any valid variable
  name along with any of these three formats:
    - ```java
      String[] args
      String options[]
      String… friends
      ```
      - The characters `[]` are brackets and represent an array. An array
      is a fixed-size list of items that are all of the same type. The
      characters `…` are called varargs (variable argument lists).
- Examples of a valid `main()` method:
  ```java
    `public static void main(String[] args){ }`
    `public static void main(String... args){ }`
    `public static void main(String[] args) throws Exception{ }`
  ```
- Examples of an invalid `main()` method:
Note that all methods are valid. It is not a compilation error if 
you have these methods in your class. But they cannot be
 accepted as the "main" method.
  - ```java
    static void main(String abc[]){ }
    ```
    - Invalid because it is not `public`
  - ```java
    public void main(String []x){ }
    ```
    - Invalid because it is not `static`

  - ```java
    public static void main(String[] a,String b){ }
    ```
    - Invalid because it does'nt take exactly one parameter of type `String` array.
  - ```java
    static void Main(String[] args){ }
    ```
    - Invalid because it is not `public` and the name starts with a capital M. 
    Remember that Java is case sensitive.
- Optional Modifiers in `main()` Methods
  - While most modifiers, such as `public` and `static`, are required for `main()` 
  methods, there are some optional modifiers allowed.
    <br/>
    ```java
    public final static void main(final String[] args){}
    ``` 
    <br/>

    Both `final` modifiers are optional, and the `main()` method is a valid entry 
    point with or without them.