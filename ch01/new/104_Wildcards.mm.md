---

markmap:
  initialExpandLevel: 1
---
# **Wildcards**
- You can use a shortcut to import all the classes in a package.
  - The `*` is a wildcard that matches all classes in the package.
- ```
  import java.util.*; //imports java.util.Random among other things
  public class NumberPicker{
    public static void main(String[] args) {
      Random r = new Random();
      System.out.println(r.nextInt(10));
    }
  }
  ```
  - We imported `java.util.Random` and a pile of other classes. 
  - Every class in the `java.util` package is available to this program
when Java compiles it.
  - The `import` statement doesn’t bring in child packages, fields,
  or methods; it imports only classes directly under the package. 
  - Let’s say you wanted to use the class `AtomicInteger` in the 
  `java.util.concurrent.atomic` package. Which import or
   imports support this?
    ```java
    import java.util.*;
    import java.util.concurrent.*;
    import java.util.concurrent.atomic.*;
    ```
    - Only the last import allows the class to be recognized because 
    child packages are not included with the first two.
  - You might think that including so many classes slows down your program 
  execution, but it doesn’t. The compiler figures out what’s actually needed. 
  Which approach you choose is personal preference or team preference.
    - Listing the classes used makes the code easier
    to read, especially for new programmers
- **Redundant Imports**
  - `java.lang` package is special in that it is automatically imported. You can 
  type this package in an import statement, but it is not necessary.
    - Another case of redundancy involves importing a class that is in the
    same package as the class importing it. Java automatically looks in
    the current package for other classes.
  - How many of the imports do you think are redundant?
    ```java
    1: import java.lang.System;
    2: import java.lang.*;
    3: import java.util.Random;
    4: import java.util.*;
    5: public class NumberPicker {
    6:   public static void main(String[] args) {
    7:     Random r = new Random();
    8:     System.out.println(r.nextInt(10));
    9:   }
    10: }
    ```
    - The answer is that three of the imports are redundant. Lines 1 and 2 are
     redundant because everything in `java.lang` is automatically imported. 
    Line 4 is also redundant because `Random` is already imported from 
    `java.util.Random`. If line 3 wasn’t present, `java.util.*` wouldn’t be
     redundant, though, since it would cover importing `Random`.
  - `Files` and `Paths` are both in the package `java.nio.file`. Which import
  statements do you think would work to get this code to compile?
    ```java
    public class InputImports {
      public void read(Files files) {
        Paths.get("name");
      }
    }
    ```
    - The shorter one is to use a wildcard to import both at the same time.
      ```java
      import java.nio.file.*;
      ```
    - The other answer is to import both classes explicitly.
      ```java
      import java.nio.file.Files;
      import java.nio.file.Paths;`
      ```
    - Now let’s consider some imports that don’t work.
      ```java
      import java.nio.*;  //NO GOOD - a wildcard only matches`
                          //class names, not "file.Files"
      import java.nio.*.*;  //NO GOOD - you can only have one
                            //wildcard and it must be at the end`
      import java.nio.file.Paths.*; //NO GOOD - you cannot import
                                    //methods only class names
      ```
- **Naming Conflicts**
  - You’ll sometimes want to import a class that can be found in multiple
  places. A common example of this is the `Date` class. Java provides 
  implementations of `java.util.Date` and `java.sql.Date`. 
  - What `import` statement can we use if we want the `java.util.Date` version?
    ```java
    public class Conflicts{
      Date date;
      //some more code
    }
    ```
    -  You can write either `import java.util.*;` or `import java.util.Date;`
  - The tricky cases come about when other imports are present.
    ```java
    import java.util.*;
    import java.sql.*; // causes Date declaration to not compile
    ```
    - When the class name is found in multiple
    packages, Java gives you a compiler error. 
  - But what do we do if we need a whole pile of other classes in the `java.sql` 
  package?
    ```java
    import java.util.Date;
    import java.sql.*;
    ```
    Now it works! **If you explicitly import a class name, it takes precedence 
    over any wildcards present.**

  - One more example. What does Java do with “ties” for precedence?
    ```java
    import java.util.Date;`
    `import java.sql.Date;`
    ```
    Java is smart enough to detect that this code is no good. The compiler 
    tells you the imports are ambiguous.
    - **If You Really Need to Use Two Classes with the Same Name**.
You can pick one to use in the `import` statement and use the other’s fully
qualified class name. Or you can always use the fully qualified class name.
      ```java
      public class Conflicts {
        java.util.Date date;
        java.sql.Date sqlDate;
      }
      ```