---

markmap:
  initialExpandLevel: 1
---
# **Executing Instance Initializer Blocks**
- Sometimes code blocks are inside a method. 
These are run when the method is called
  - Anywhere you see braces is a code block.
  - Instance initializers are code blocks appear outside a method 
    - Instance initializers cannot exist inside a method.
- How many blocks do you see in the following example? 
How many instance initializers do you see?
  - ```java
    1: public class Bird {
    2:   public static void main(String[] args) {
    3:     { System.out.println("Feathers"); }
    4:   }
    5:   { System.out.println("Snowy"); }
    6: }
    ```
    - There are four code blocks in this example: a class
    definition, a method declaration, an inner block, and
    an instance initializer.
- **Following the Order of Initialization**
  - Later, we'll add some additional rules to the initialization
  order. In the meantime, remember the following: <br/>
  1.- Fields and instance initializer blocks are run in 
  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;the order in which they appear in the file.
  2.- The constructor runs after all fields and instance
   &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;initializer blocks have run. 
    - Example:
      ```java
      1:  public class Chick{
      2:    private String name = "Fluffy";
      3:    { System.out.println("setting field"); }
      4:    public Chick() {
      5:       name = "Tiny";
      6:       System.out.println("setting constructor");
      7:    }
      8:    public static void main(String[] args) {
      9:       Chick chick = new Chick();
      10:      System.out.println(chick.name); } }
      ```
      - It prints the following:
        ```
        setting field
        setting constructor
        Tiny
        ```
      - We start with the `main()` method because that’s where Java starts
      execution. On line 9, we call the constructor of `Chick`. Java creates a
      new object. First it initializes `name` to `"Fluffy"` on line 2. Next it executes
      the `println()` statement in the instance initializer on line 3. Once all the
      fields and instance initializers have run, Java returns to the constructor. 
      Line 5 changes the value of `name` to `"Tiny"`, and  line 6 prints another
      statement. At this point, the constructor is done,  and then the execution 
      goes back to the `println()` statement on line 10.
  -  You can’t read a variable before it has been defined.
      - ```java
        { System.out.println(name); } // DOES NOT COMPILE
        {name = "name";} // It is ok
        private String name = "Fluffy";
        ```
  -  What do you think this code prints out?
      - ```java
        public class Egg {
        public Egg() {
          number = 5;
        }
        public static void main(String[] args) {
          Egg egg = new Egg();
          System.out.println(egg.number);
        }
        private int number = 3;
        { number = 4; } }
        ```
        - It prints:
        `5`
        - If you answered `5`, you got it right. Fields and blocks 
        are run first in order, setting `number` to `3` and then `4`. 
        Then the constructor runs, setting `number` to `5`. 