---

markmap:
  initialExpandLevel: 1
---
# **Declaring Constructors**
- A constructor is a special method that matches the name of the class and has 
no return type. It is called when a new instance of the class is created.
  - **Creating a Constructor**
    Let’s start with a simple constructor:
    ```java
    public class Bunny {
      public Bunny() {
        System.out.print("hop");
      }
    }
    ```
    The name of the constructor, `Bunny`, matches the name of
    the class, `Bunny`, and there is no return type, not even `void`.
    That makes this a constructor. 
      -  Can you tell why these two are not valid constructors for the `Bunny` class?
          ```java
          public class Bunny {
            public bunny() {} // DOES NOT COMPILE
            public void Bunny() {}
          }
          ```
          The first one doesn’t match the class name because Java is case-sensitive. 
          Since it doesn’t match, Java knows it can’t be a constructor and is supposed 
          to be a regular method. However, it is missing the return type and doesn’t 
          compile. The second method is a perfectly good method but is not a 
          constructor because it has a return type.
            - Like method parameters, constructor parameters can be
            any valid class, array, or primitive type, including generics,
            but may not include `var`.
              ```java
              public class Bonobo {
                public Bonobo(var food) { // DOES NOT COMPILE
                }
              }
              ```
              - A class can have multiple constructors, as long as each
              constructor has a unique constructor signature. In this
              case, that means the constructor parameters must be
              distinct. Like methods with the same name but different
              signatures, declaring multiple constructors with different
              signatures is referred to as _constructor overloading_.
                -  The following `Turtle` class has four distinct overloaded constructors:
                    ```java
                    public class Turtle {
                      private String name;
                      public Turtle() {
                        name = "John Doe";
                      }
                      public Turtle(int age) {}
                      public Turtle(long age) {}
                      public Turtle(String newName, String… favoriteFoods) {
                        name = newName;
                      }
                    }
                    ```
                    -  A constructor is called when we write `new` followed by the 
                    name of the class we want to  instantiate. Here’s an example:
                      `new Turtle(15)`
                      When Java sees the `new` keyword, it allocates memory for
                      the new object. It then looks for a constructor with a matching 
                      signature and calls it.
- **The Default Constructor**
If you don’t include any constructors in the class, Java will
create one for you without any parameters. This Java-created
constructor is called the default constructor and is added any 
time a class is declared without any constructors. We often
refer to it as the default no-argument constructor, for clarity. 
Here’s an example:
  ```java
  public class Rabbit {
    public static void main(String[] args) {
      new Rabbit(); // Calls the default constructor
    }
  }
  ```
  - In the `Rabbit` class, Java sees that no constructor was coded and 
  creates one. The previous class is equivalent to the following, in 
  which the default constructor is provided and therefore not inserted 
  by the compiler:
    ```java
    public class Rabbit {
      public Rabbit() {}
      public static void main(String[] args) {
        new Rabbit(); // Calls the user-defined constructor
      }
    }
    ```
    - The default constructor has an empty parameter list and an
    empty body. It is fine for you to type this in yourself.
    However, since it doesn’t do anything, Java is happy to
    generate it for you and save you some typing.
    We keep saying generated. This happens during the
    compile step. If you look at the file with the `.java` extension,
    the constructor will still be missing. It only makes an
    appearance in the compiled file with the `.class` extension.
      - Which of these classes do you think has a default constructor?
        ```java
        public class Rabbit1 {}
        public class Rabbit2 {
          public Rabbit2() {}
        }
        public class Rabbit3 {
          public Rabbit3(boolean b) {}
        }
        public class Rabbit4 {
          private Rabbit4() {}
        }
        ```
        - Only `Rabbit1` gets a default no-argument constructor. It
        doesn’t have a constructor coded, so Java generates a
        default no-argument constructor. `Rabbit2` and `Rabbit3` both
        have public constructors already. `Rabbit4` has a `private`
        constructor. Since these three classes have a constructor
        defined, the default no-argument constructor is not
        inserted for you.
          - Let’s take a quick look at how to call these constructors:
            ```java
            1: public class RabbitsMultiply {
            2:   public static void main(String[] args) {
            3:     var r1 = new Rabbit1();
            4:     var r2 = new Rabbit2();
            5:     var r3 = new Rabbit3(true);
            6:     var r4 = new Rabbit4(); // DOES NOT COMPILE
            7: } }
            ```
            Line 3 calls the generated default no-argument constructor.
            Lines 4 and 5 call the user-provided constructors. Line 6
            does not compile. `Rabbit4` made the constructor private so
            that other classes could not call it.
              - **Calling Overloaded Constructors with _`this()`_**
                ```java
                public class Hamster {
                  private String color;
                  private int weight;
                  public Hamster(int weight, String color) {
                    // First constructor
                    this.weight = weight;
                    this.color = color;
                  }
                  public Hamster(int weight) {
                    // Second constructor
                    this(weight, "brown");
                  }
                }
                ```
                - When `this()` is used with parentheses, Java calls another
                constructor on the same instance of the class.
- **_`this`_ vs. _`this()`_**
The first, `this`, refers to an instance of the class, while the second, 
`this()`, refers to a constructor call within the class. 
  - The `this()` call must be the first statement in the constructor. 
  You can only have one call to `this()` in any constructor.
    ```java
    3: public Hamster(int weight) {
    4:   System.out.println("chew");
    5:   // Set weight and default color
    6:   this(weight, "brown"); // DOES NOT COMPILE
    7: }
    ```
    - Even though a print statement on line 4 doesn’t change any
    variables, it is still a Java statement and is not allowed to be
    inserted before the call to `this()`. The comment on line 5 is
    just fine. Comments aren’t considered statements and are
    allowed anywhere.
      - There’s one last rule for overloaded constructors that you
      should be aware of. Consider the following definition of the
      `Gopher` class:
        ```java
        public class Gopher {
          public Gopher(int dugHoles) {
            this(5); // DOES NOT COMPILE
          }
        }
        ```
        The compiler is capable of detecting that this constructor is
        calling itself infinitely (in loops the compiler does not detect it). 
          - This also does not compile.
            ```java
            public class Gopher {
              public Gopher() {
                this(5); // DOES NOT COMPILE
              }
              public Gopher(int dugHoles) {
                this(); // DOES NOT COMPILE
              }
            }
            ```
            In this example, the constructors call each other, and the
            process continues infinitely. Since the compiler can detect
            this, it reports an error.
              - Here we summarize the rules you should know about
              constructors that we covered in this section.
                - 1.- A class can contain many overloaded constructors,
                provided the signature for each is distinct.
                2.- The compiler inserts a default no-argument constructor
                if no constructors are declared.
                3.- If a constructor calls `this()`, then it must be the first
                line of the constructor.
                4.- Java does not allow cyclic constructor calls.
- **Calling Parent Constructors with _`super()`_**
The first statement of every constructor is a call to a parent
constructor using `super()` or another constructor in the class 
using `this()`.
  - Let’s take a look at the `Animal` class and its subclass `Zebra`
  and see how their constructors can be properly written to
  call one another:
    - ```java
      public class Animal {
        private int age;
        public Animal(int age) {
          //Refers to constructor in java.lang.Object
          super();
          this.age = age;
        }
      }
      public class Zebra extends Animal {
        public Zebra(int age) {
          super(age); //Refers to constructor: Animal(int)
        }
        public Zebra() {
        //Refers to constructor: Zebra( int )
          this(4);
        }
      }
      ```
      - In the `Animal` class, the first statement of the constructor is
      a call to the parent constructor defined in `java.lang.Object`,
      which takes no arguments. In the second class, `Zebra`, the
      first statement of the first constructor is a call to `Animal`’s
      constructor, which takes a single argument. The `Zebra` class
      also includes a second no-argument constructor that
      doesn’t call `super()` but instead calls the other constructor
      within the `Zebra` class using `this(4)`.
        - **_`super`_ vs. _`super()`_**
          The first, `super`, is used to reference members of the parent 
          class, while the second, `super()`, calls a parent constructor.
            - Like calling `this()`, calling `super()` can only be 
            used as the first statement of the constructor.
              ```java
              public class Zoo {
                public Zoo() {
                  System.out.println("Zoo created");
                  super(); //DOES NOT COMPILE
                }
              }
              public class Zoo {
                public Zoo() {
                  super();
                  System.out.println("Zoo created");
                  super(); //DOES NOT COMPILE
                }
              }
              ```
              - The first class will not compile because the call to the parent constructor 
              must be the first statement of the constructor. In the second code snippet,
              `super()` is the first statement of the constructor, but it is also used as the 
              third statement. Since `super()` can only be called once as the first statement 
              of the constructor, the code will not compile.
- If the parent class has more than one constructor, the child
class may use any valid and accessible parent constructor
in its definition, as shown in the following example:
  - ```java
    public class Animal {
      private int age;
      private String name;
      public Animal(int age, String name) {
        super();
        this.age = age;
        this.name = name;
      }
      public Animal(int age) {
        super();
        this.age = age;
        this.name = null;
      }
    }
    public class Gorilla extends Animal {
      public Gorilla(int age) {
        // Calls the first Animal constructor
        super(age, "Gorilla"); 
      }
      public Gorilla() {
        // Calls the second Animal constructor
        super(5); 
      }
    }
    ```
    - In this example, the first child constructor takes one
argument, `age`, and calls the parent constructor, which
takes two arguments, `age` and `name`. The second child
constructor takes no arguments, and it calls the parent
constructor, which takes one argument, `age`. In this
example, notice that the child constructors are not required
to call matching parent constructors. Any valid parent
constructor is acceptable as long as it is accessible and the
appropriate input parameters to the parent constructor are
provided.