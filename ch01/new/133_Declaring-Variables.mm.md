---

markmap:
  initialExpandLevel: 1
---
# **Declaring Variables**
- A _variable_ is a name for a piece of memory that stores data.
  - To declare a variable you have to provide the type and name
  - Giving a variable a value is called _initializing_ a variable.  You
  just type the variable name followed by an equal sign,
  followed by the desired value.
    - The code declares and initializes the `zooName` variable.
      ```java
      String zooName = "The Best Zoo";
      ```
- **Identifying Identifiers**
  - An _identifier_ is the name of a variable, method,
  class, record, interface, or package.
    - There are four rules to remember for legal identifiers.
      - Identifiers must begin with a letter, a currency symbol, or a _
      symbol. Currency symbols include dollar (`$`), yuan (`¥`),
      euro (`€` ), and so on.
      - Identifiers can include numbers but not start with them.
      - A single underscore (`_`) is not allowed as an identifier.
      - You cannot use the same name as a Java reserved word.
      But you can use contextual keywords.
    - In Java, contextual keywords are identifiers that have a special meaning in certain contexts but can be used as regular identifiers in others.
    Some of the contextual keywords include `var`, `yield`, `sealed`, `non-sealed`, `permits`, `record`, `transitive`, `with`, `provides`, `to`, `opens`,
    `exports`, `requires`, and `module` (note that they are all lowercase). These keywords can be used in various contexts such as method names,
    variable names, and package names, but they are restricted from being used as class names, interface names, records names or enum names.
  - The following examples are legal:
    - These examples are not legal:
  - ```java
    long okidentifier;
    float $OK2Identifier;
    boolean _alsoOK1d3ntifi3r;
    char __SStillOkbutKnotsonice$;
    ```
    - ```java
      int 3DPointClass;    // identifiers cannot begin with a number
      byte hollywood@vine; //@ is not a letter, digit, $ or _
      String *$coffee;     //first character * is not a letter, $ or _
      double public;       //public is a reserved word
      short _;             //a single underscore is not allowed
      ```
- **Declaring Multiple Variables**
  - How many variables do you think are declared and
   initialized in the following example?
    - ```java
      void sandFence() {
        String s1, s2;
        String s3 = "yes", s4 = "no";
      }
      ```
      - Four `String` variables were declared: `s1`, `s2`, `s3`, and `s4`.  You can
      declare many variables in the same declaration as long as they are
      all of the same type. You can also initialize any or all of those values
      inline. In the example, we have two initialized variables: `s3` and `s4`.
      The other two variables remain declared but not yet initialized.
    - ```java
      void paintFence() {
        int i1, i2, i3 = 0;
      }
      ```
      - Three variables were declared: `i1`, `i2`, and `i3`.
      However, only one of those variables was initialized: `i3`.
    - ```java
      int num, String value; // DOES NOT COMPILE
      ```
      - This code doesn’t compile because it tries to declare multiple
      variables of different types in the same statement. The shortcut to
      declare multiple variables in the same statement is legal only when
      they share a type.
  - See if you can figure out which of
   the following are legal declarations:
    - ```java
      4: boolean b1, b2;
      5: String s1 = "1", s2;
      6: double d1, double d2;
      7: int i1; int i2;
      8: int i3; i4;
      ```
      - Lines 4 and 5 are legal. They each declare two variables. Line 4 doesn’t initialize either variable, and
      line 5 initializes only one. Line 7 is also legal. Although `int` does appear twice, each one is in a
      separate statement. A semicolon (`;`) separates statements in Java. It just so happens there are two
      completely different statements on the same line.
      Line 6 is not legal. Java does not allow you to declare two different types in the same statement. If 
      you want to declare multiple variables in the same statement, they must share the same type
      declaration and not repeat it.
      Line 8 is not legal. Again, we have two completely different statements on the same line. The second
      one on line 8 is not a valid declaration because it omits the type. 