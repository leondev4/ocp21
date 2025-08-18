---

markmap:
  initialExpandLevel: 1
---
# **Ordering Elements in a Class**
- Comments can go anywhere in the code. 
-  Order for declaring a class
**Table 1.4**
    - **TABLE 1.4** Order for declaring a class
      - The first example contains one of each element.
        |Element|Example|Required?|Where does it go?|
        |-|-|-|-|
        |Package declaration|`package abc;`|No|First line in the file (excluding ­comments or blank lines)|
        |`import` statements|`import java.util.*;`|No|Immediately after the package ­(if present)|
        |Top-­level type declaration|`public class C`|Yes|Immediately after the import (if any)|
        |Field declarations|`int value;`|No|Any top-­level element within a class|
        |Method declarations|`void method()`|No|Any top-­level element within a class|
  - ```java
    package structure; //package must be first non-comment
    import java.util.*; //import must come after package
    public class Meerkat {// then comes the class
      double weight;// fields and methods can go in either order
      public double getWeight() {
        return weight; }
      double height; // another field - they don't need to be together
    }
    ```
-  How about this one?
    - ```java
      /* header */

      package structure;

      // class Meerkat
      public class Meerkat{ }
      ```
      - Still good. We can put comments anywhere, blank
     lines are ignored, and imports are optional.
-  In the next example, we have a problem:
    - ```java
      import java.util.*;
      package structure; //DOES NOT COMPILE
      String name; //DOES NOT COMPILE
      public class Meerkat { } // DOES NOT COMPILE
      ```
      - There are two problems here. One is that the `package` and `import` statements
      are reversed. Although both are optional, `package` must come before `import` 
      if present. The other issue is that a field attempts a declaration outside a class. 
      This is not allowed. Fields and methods must be within a class.
- **Think of the acronym PIC (picture): `package`, `import`,
and `class`. Fields and methods must be inside a class.**