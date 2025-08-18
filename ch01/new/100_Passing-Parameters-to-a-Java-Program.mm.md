---

markmap:
  initialExpandLevel: 1
---
# **Passing Parameters to a Java Program**
- We modify the Zoo program to print out the first two
 arguments passed in.
  ```java
  public class Zoo {
    public static void main(String[] args) {
      System.out.println(args[0]);
      System.out.println(args[1]);
    }
  }
  ```
  - The code `args[0]` accesses the first element of the array. 
  That’s right: array indexes begin with `0` in Java.
    - To run it, type this:
      ```java
      javac Zoo.java
      java Zoo Bronx Zoo
      ```
      - The output is what you might expect.
        ```java
        Bronx
        Zoo
        ```
        - The program correctly identifies the first two "words" as the
        arguments. Spaces are used to separate the arguments. 
- If you want spaces inside an argument, you 
need to use quotes as in this example:
  ```java
  javac Zoo.java
  java Zoo "San Diego" Zoo
  ```
  - Now we have a space in the output.
    ```java
    San Diego
    Zoo
    ```

- What happens if you don’t pass in enough arguments?
  ```java
  javac Zoo.java
  java Zoo Zoo
  ```
  - Java prints out an exception telling you it has no idea what to do with this argument at position 1.
    ```java
    Zoo
    Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException:
      Index 1 out of bounds for length 1
      at Zoo.main(Zoo.java:4)
    ```
- **Single-File Source Code**
If you get tired of typing both `javac` and `java` every time you want
to try a code example, there’s a shortcut. You can instead run this:
  ```java
  java Zoo.java Bronx Zoo
  ```
  - There is a key difference here. When compiling first, you
  omitted the file extension when running `java`. When skipping
  the explicit compilation step, you include this extension. This
  feature is called launching _single-file source-code_ programs.