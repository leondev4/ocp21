---

markmap:
  initialExpandLevel: 1
---
# **Understanding Package<br/> Declarations and Imports**
-  Java puts classes in _packages_. These are logical groupings for classes. 
Java needs you to tell it which packages to look in to find code.
    - Suppose you try to compile this code:
        ```java
        public class NumberPicker{
          public static void main(String[] args){
            Random r = new Random(); // DOES NOT COMPILE
            System.out.println(r.nextInt(10));
          }
        }
        ```
        - The Java compiler helpfully gives you an error
          ```java
          error: cannot find symbol
          ```
        - The error is omitting an _import_ statement.
        `import` statements tell Java which packages 
        to look in for classes.
    - Trying this again with the `import` allows the code to compile.
    - ```java
      import java.util.Random; //import tells Java where to find Random
      public class NumberPicker {
        public static void main(String[] args) {
          Random r = new Random();
          System.out.println(r.nextInt(10)); // a number 0-9
        }
      }
      ```
      - It prints out a random number between `0` and `9`.
      Just like arrays, Java likes to begin counting with `0`.

    - The `import` statement tells the compiler which package to
    look in to find a class.
      
- **Packages**
  - You start reading a package name at the beginning. 
  For example, if it begins with `java`, this means it 
  came with the JDK. If it starts with something else,
   it likely shows where it came from using the website 
   name in reverse.  For example, `com.wiley.javabook`.
   -  Java calls more detailed packages child packages.
    The package `com.wiley.javabook` is a child package 
    of `com.wiley`. 
    - The rule for package names is that they are mostly
    letters or numbers separated by periods (`.`).
    Technically, you’re allowed a couple of other 
    characters between the periods (`.`). The rules are
    the same as for variable names.