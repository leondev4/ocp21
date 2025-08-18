# **Initializing Classes**
- First, we initialize the class, which involves invoking all `static` members in the class hierarchy, 
starting with the highest superclass and working downward. This is sometimes referred to as 
_loading the class_. The class may be initialized when the program first starts, when a `static`
member of the class is referenced, or shortly before an instance of the class is created.
One of the most important rules with class initialization is that it happens at most once for 
each class. The class may also never be loaded if it is not used in the program.
  - **Initialize Class X**
  1.- Initialize the superclass of X.
  2.- Process all `static` variable declarations in the order in which they appear in the class.
  3.- Process all `static` initializers in the order in which they appear in the class.
    - Taking a look at an example, what does the following program print?
      ```java
      public class Animal {
        static { System.out.print("A"); }
      }
      public class Hippo extends Animal {
        public static void main(String[] grass) {
          System.out.print("C");
          new Hippo();
          new Hippo();
          new Hippo();
        }
        static { System.out.print("B"); }
      }
      ```
      - It prints `ABC` exactly once. Since the `main()` method is inside the `Hippo` class, 
      the class will be initialized first, starting with the superclass and printing `AB`. 
      Afterward, the `main()` method is executed, printing `C`. **Even though the 
      `main()` method creates three instances, the class is loaded only once.**
        - **Why the Hippo Program Printed `C` After `AB`**
          In the previous example, the `Hippo` class was initialized before the `main()` 
          method was executed. This happened because our `main()` method was 
          inside the class being executed, so it had to be loaded on startup. What if 
          you instead called `Hippo` inside another program?
          ```java
          public class HippoFriend {
            public static void main(String[] grass) {
              System.out.print("C");
              new Hippo();
            }
          }
          ```
          - Assuming the class isn’t referenced anywhere else, this
          program will likely print `CAB`, with the `Hippo` class not
          being loaded until it is needed inside the `main()` method.
- **Initializing _`final`_ Fields**
Instance and class variables are assigned a default value based on their type if no value is 
specified. For example, a `double` is initialized with `0.0`, while an object reference is initialized 
to `null`. A default value is only applied to a non-`final` field, though
  - `final static` variables must be explicitly assigned a value exactly once. Fields marked
  `final` follow similar rules. They can be assigned values in the line in which they are 
  declared, in an instance initializer or constructor.
    ```java
    public class MouseHouse {
      private final int volume;
      private final String name = "The Mouse House"; //Declaration assignment
      {
        volume = 10; //Instance initializer assignment
      }
    }
    ```
    - `final` instance fields can also be set in a constructor. The constructor 
    is part of the initialization process, so it is allowed to assign `final`
    instance variables. You need to know one important rule: _by the time 
    the constructor completes, all `final` instance variables must be 
    assigned a value exactly once_.
      - Let’s try this in an example:
        ```java
        public class MouseHouse {
          private final int volume;
          private final String name;
          public MouseHouse() {
            // Constructor assignment
            this.name = "Empty House"; 
          }
          {
            // Instance initializer assignment
            volume = 10; 
          }
        }
        ```
        - Unlike local `final` variables, which are not required to have a value unless 
        they are actually used, `final` instance variables must be assigned a value. 
        If they are not assigned a value when they are declared or in an instance 
        initializer, then they must be assigned a value in the constructor declaration. 
        Failure to do so will result in a compiler error.
          - ```java
            public class MouseHouse {
              private final int volume;
              private final String type;
              {
                this.volume = 10;
              }
              public MouseHouse(String type) {
                this.type = type;
              }
              public MouseHouse() { // DOES NOT COMPILE
                this.volume = 2; // DOES NOT COMPILE
              }
            }
            ```
            - In this example, the first constructor that takes a `String` argument compiles. In terms 
            of assigning values, each constructor is reviewed individually, which is why the
            second constructor does not compile. First, the constructor fails to set a value for the 
            `type` variable. The compiler detects that a value is never set for `type` and reports an
            error. Second, the constructor sets a value for the `volume` variable, even though it 
            was already assigned a value by the instance initializer. 
- On the exam, be wary of any instance variables marked `final`. Make sure they are assigned 
a value in the line where they are declared, in an instance initializer, or in a constructor. They 
should be assigned a value only once, and failure to assign a value is considered a compiler 
error.
  - What about `final` instance variables when a constructor calls another constructor in the 
  same class? In that case, you have to follow the flow carefully, making sure every `final` 
  instance variable is assigned a value exactly once. We can replace our previous bad 
  constructor with the following one that does compile:
    ```java
    public MouseHouse() {
    this(null);
    }
    ```
    - This constructor does not perform any assignments to any `final` instance 
    variables, but it calls the `MouseHouse(String)` constructor, which we 
    observed compiles without issue. We use `null` here to demonstrate that 
    the variable does not need to be an object value. We can assign a `null` 
    value to `final` instance variables as long as they are explicitly set.
      - **Initializing Instances**
      First, start at the lowest-level constructor where the `new`
      keyword is used. Remember, the first line of every
      constructor is a call to `this()` or `super()`, and if omitted, the
      compiler will automatically insert a call to the parent no-
      argument constructor `super()`. Then, progress upward and
      note the order of constructors. Finally, initialize each class
      starting with the superclass, processing the instance
      initializers and constructors within each class, in the
      reverse order in which each class was instantiated. 
        - Initialize Instance of X
          1.- Initialize Class X if it has not been previously initialized.
          2.- Initialize the superclass instance of X.
          3.- Process all instance variable declarations in the order in which they appear in the class.
          4.- Process all instance initializers in the order in which they appear in the class.
          5.- Initialize the constructor, including any overloaded constructors referenced with `this()`.
            - Let’s try an example with no inheritance. See if you can figure out what 
            the following application outputs:
              ```java
              1: public class ZooTickets {
              2:   private String name = "BestZoo";
              3:   { System.out.print(name + "-"); }
              4:   private static int COUNT = 0;
              5:   static { System.out.print(COUNT + "-"); }
              6:   static { COUNT += 10; System.out.print(COUNT + "-"); }
              7:
              8:   public ZooTickets() {
              9:     System.out.print("z-");
              10:  }
              11:
              12:  public static void main(String… patrons) {
              13:    new ZooTickets();
              14: } }
              ```
              - The output is as follows:
                `0-10-BestZoo-z-`
                First, we have to initialize the class. Since there is no superclass declared, which 
                means the superclass is `Object`, we can start with the `static` components of
                `ZooTickets`. In this case, lines 4, 5, and 6 are executed, printing `0-` and `10-`.
                Next, we initialize the instance created on line 13. Again, since no superclass is
                declared, we start with the instance components. Lines 2 and 3 are executed, 
                which prints `BestZoo-`. Finally, we run the constructor on lines 8–10, which outputs 
                `z-`.
                
- Let’s try a simple example with inheritance:
  ```java
  class Primate {
    public Primate() {
      System.out.print("Primate-");
    } }
  class Ape extends Primate {
    public Ape(int fur) {
      System.out.print("Ape1-");
    }
    public Ape() {
      System.out.print("Ape2-");
    } }
  public class Chimpanzee extends Ape {
    public Chimpanzee() {
      super(2);
      System.out.print("Chimpanzee-");
    }
    public static void main(String[] args) {
      new Chimpanzee();
  } }
  ```
  - The compiler inserts the `super()` command as the first
    statement of both the `Primate` and `Ape` constructors. The
    code will execute with the parent constructors called first
    and yield the following output:
    `Primate-Ape1-Chimpanzee-`
    Notice that only one of the two `Ape()` constructors is called.
    You need to start with the call to `new Chimpanzee()` to
    determine which constructors will be executed. Remember,
    constructors are executed from the bottom up, but since
    the first line of every constructor is a call to another
    constructor, the flow ends up with the parent constructor
    executed before the child constructor.
      - The next example is a little harder. What do you think
        happens here?
        ```java
        1: public class Cuttlefish {
        2:   private String name = "swimmy";
        3:   { System.out.println(name); }
        4:   private static int COUNT = 0;
        5:   static { System.out.println(COUNT); }
        6:   { COUNT++; System.out.println(COUNT); }
        7:
        8:   public Cuttlefish() {
        9:     System.out.println("Constructor");
        10:  } 
        11:
        12:  public static void main(String[] args) {
        13:    System.out.println("Ready");
        14:    new Cuttlefish();
        15: } }
        ```
        - The output looks like this:
          ```java
          0
          Ready
          swimmy
          1
          Constructor
          ```
          No superclass is declared, so we can skip any steps that
          relate to inheritance. We first process the `static` variables
          and `static` initializers—lines 4 and 5, with line 5 printing `0`.
          Now that the `static` initializers are out of the way, the `main()`
          method can run, which prints `Ready`. Next we create an
          instance declared on line 14. Lines 2, 3, and 6 are
          processed, with line 3 printing `swimmy` and line 6 printing `1`.
          Finally, the constructor is run on lines 8–10, which prints
          `Constructor`.
          - Ready for a more difficult example, the kind you might see
          on the exam? What does the following output?
            - ```java
              1: class GiraffeFamily {
              2:   static { System.out.print("A"); }
              3:   { System.out.print("B"); }
              5:   public GiraffeFamily(String name) {
              6:     this(1);
              7:     System.out.print("C");
              8:   }
              10:   public GiraffeFamily() {
              11:     System.out.print("D");
              12:   }
              14:   public GiraffeFamily(int stripes) {
              15:     System.out.print("E");
              16:   }
              17: }
              18: public class Okapi extends GiraffeFamily {
              19:   static { System.out.print("F"); }
              21:   public Okapi(int stripes) {
              22:     super("sugar");
              23:     System.out.print("G");
              24:   }
              25:   { System.out.print("H"); }
              27:   public static void main(String[] grass) {
              28:     new Okapi(1);
              29:     System.out.println();
              30:     new Okapi(2);
              31:   }
              32: }
              ```
              - The program prints the following:
                ```java
                AFBECHG
                BECHG
                ```
                Let’s walk through it. Start with initializing the `Okapi` class.
                Since it has a superclass `GiraffeFamily`, initialize it first,
                printing `A` on line 2. Next, initialize the `Okapi` class, printing
                `F` on line 19.
                After the classes are initialized, execute the `main()` method
                on line 27. The first line of the main() method creates a new
                `Okapi` object, triggering the instance initialization process.
                Per the second rule, the superclass instance of `GiraffeFamily`
                is initialized first. Per our fourth rule, the instance
                initializer in the superclass `GiraffeFamily` is called, and `B` is
                printed on line 3. Per the fifth rule, we initialize the
                constructors. In this case, this involves calling the
                constructor on line 5, which in turn calls the overloaded
                constructor on line 14. The result is that `EC` is printed, as
                the constructor bodies are unwound in the reverse order
                that they were called.
                  - The process then continues with the initialization of the
                  `Okapi` instance itself. Per the fourth and fifth rules, `H` is
                  printed on line 25, and `G` is printed on line 23, respectively.
                  The process is a lot simpler when you don’t have to call any
                  overloaded constructors. Line 29 then inserts a line break
                  in the output. Finally, line 30 initializes a new `Okapi` object.
                  The order and initialization are the same as line 28, sans
                  the class initialization, so `BECHG` is printed again. Notice that
                  `D` is never printed, as only two of the three constructors in
                  the superclass `GiraffeFamily` are called.
                    - This example is tricky for a few reasons. There are multiple
                    overloaded constructors, lots of initializers, and a complex
                    constructor pathway to keep track of. Luckily, questions
                    like this are uncommon on the exam. If you see one, just
                    write down what is going on as you read the code.
- We conclude this section by listing important rules you
should know for the exam:
  - A class is initialized at most once by the JVM before it is
referenced or used.
  - All `static final` variables must be assigned a value
exactly once, either when they are declared or in a
`static` initializer.
  - All `final` fields must be assigned a value exactly once,
either when they are declared, in an instance initializer,
or in a constructor.
  - Non-`final static` and instance variables defined without
a value are assigned a default value based on their
type.
  - The order of initialization is as follows: variable
declarations, then initializers, and finally constructors.