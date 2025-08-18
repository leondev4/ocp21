---

markmap:
  initialExpandLevel: 1
---
# **Accessing `static` Data**
- When the `static` keyword is applied to a variable, method,
or class, it belongs to the class rather than a specific
instance of the class. 
- **Designing `static` Methods and Variables**
  -  Methods and variables declared `static` don’t require an 
  instance of the class. They are shared among all users 
  of the class.
      - ```java
        public class Penguin {
          String name;
          static String nameOfTallestPenguin;
        }
        ```
        In this class, every Penguin instance has its own `name` like
        `Willy` or `Lilly`, but only one `Penguin` among all the instances
        is the tallest. You can think of a `static` variable as being a
        member of the single class object that exists independently
        of any instances of that class
          - Consider the following example:
            ```java
            public static void main(String[] unused) {
              var p1 = new Penguin();
              p1.name = "Lilly";
              p1.nameOfTallestPenguin = "Lilly";
              var p2 = new Penguin();
              p2.name = "Willy";
              p2.nameOfTallestPenguin = "Willy";

              System.out.println(p1.name);                 // Lilly
              System.out.println(p1.nameOfTallestPenguin); // Willy
              System.out.println(p2.name);                 // Willy
              System.out.println(p2.nameOfTallestPenguin); // Willy
            }
            ```
            - We see that each penguin instance is updated with its own
            unique `name`. The `nameOfTallestPenguin` field is `static` 
            and therefore shared, though, so anytime it is updated, it
            impacts all instances of the class.
              - The `main()` method is a `static` method. That means you can call it
              using the class name:
                ```java
                public class Koala {
                  public static int count = 0;  //static  variable
                  public static void main(String[] a) {//static method
                    System.out.print(count);
                  }
                }
                ```
                - Here the JVM basically calls `Koala.main()` to get the program
                started. You can do this too. We can have a `KoalaTester` that
                does nothing but call the `main()` method:
                  ```java
                  public class KoalaTester {
                    public static void main(String[] args) {
                      Koala.main(new String[0]); // call static method
                    }
                  }
                  ```
                  - Quite a complicated way to print `0`, isn’t it? When we run
                  `KoalaTester`, it makes a call to the `main()` method of `Koala`,
                  which prints the value of `count`. The purpose of all these
                  examples is to show that `main()` can be called just like any
                  other `static` method.
                    - In addition to `main()` methods, `static` 
                    methods have two main purposes:
                      - For utility or helper methods that don’t require any
                      object state. Since there is no need to access instance
                      variables, having `static` methods eliminates the need
                      for the caller to instantiate an object just to call the
                      method.
                      - For state that is shared by all instances of a class, like a
                      counter. All instances must share the same state.
                      Methods that merely use that state should be `static` 
                      as well.
- **Accessing a `static` Variable or Method**
  - Usually, accessing a `static` member is easy.
    ```java
    public class Snake {
      public static long hiss = 2;
    }
    ```
    You just put the class name before the method or variable.
      ```java
      System.out.println(Snake.hiss); 
      ```
      - There is one rule that is trickier. You can use an instance of 
      the object to call a `static` method. **The compiler checks for 
      the type of the reference and uses that instead of the object**
        -  This code is perfectly legal:
            ```java
            5: Snake s = new Snake();
            6: System.out.println(s.hiss); // s is a Snake
            7: s = null;
            8: System.out.println(s.hiss); // s is still a Snake
            ```
            - This code outputs `2` twice. Line 6 sees that `s` is a `Snake` 
            and `hiss` is a `static` variable, so it reads that `static` 
            variable. Line 8 does the same thing. Java doesn’t care 
            that `s` happens to be `null`. Since we are looking for a
            `static` variable, it doesn’t matter.
              - **NOTE:**
                Remember to look at the reference type for a variable
                when you see a `static` method or variable. The exam
                creators will try to trick you into thinking a
                `NullPointerException` is thrown because the variable
                happens to be `null`. Don’t be fooled!

                - One more time, because this is really important: 
                what does the following output?
                  ```java
                  Snake.hiss = 4;
                  Snake snake1 = new Snake();
                  Snake snake2 = new Snake();
                  snake1.hiss = 6;
                  snake2.hiss = 5;
                  System.out.println(Snake.hiss)
                  ```
                  - The answer is `5`. There is only one `hiss` variable since 
                  it is `static`. It is set to `4` and then `6` and finally winds
                  up as `5`. All the `Snake` variables are just distractions.
- **Class vs. Instance Membership**
  - A `static` member cannot call an instance member without
  referencing an instance of the class.
    - Example:
      ```java
      public class MantaRay {
        private String name = "Sammy";
        public static void first() { }
        public static void second() { }
        public void third() { System.out.print(name); }
        public static void main(String args[]) {
          first();
          second();
          third(); // DOES NOT COMPILE
        }
      }
      ```
      - The compiler will give you an error about making a `static`
      reference to an instance method. If we fix this by adding
      `static` to `third()`, we create a new problem. Can you figure
      out what it is?
        ```java
        public static void third() {
          System.out.print(name); } // DOES NOT COMPILE
        ```
        All this does is move the problem. Now, `third()` is referring
        to an instance variable `name`.
          - There are two ways we could fix this. 
          The first is to add `static` to the `name` variable as well.
            ```java
            public class MantaRay {
              private static String name = "Sammy";
              …
              public static void third() { System.out.print(name); }
              …
            }
            ```
            - The second solution would have been to call `third()` as an
            instance method and not use `static` for the method or the
            variable.
              ```java
              public class MantaRay {
                private String name = "Sammy";
                …
                public void third() { System.out.print(name); }
                public static void main(String args[]) {
                  …
                  var ray = new MantaRay();
                  ray.third();
                }
              }
              ```
              - A `static` method or instance method can call a `static` method. 
              Only an instance method can call another instance method 
              on the same class without using a reference variable. Similar 
              logic applies for instance and `static` variables.
                - Suppose we have a `Giraffe` class:
                  ```java
                  public class Giraffe {
                    public void eat(Giraffe g) {}
                    public void drink() {};
                    public static void allGiraffeGoHome(Giraffe g) {}
                    public static void allGiraffeComeOut() {}
                  }
                  ```
                  - Make sure you understand **Table 5.5** before continuing.
                    - **TABLE 5.5** `static` vs. instance calls
                      |Method|Calling|Legal?|
                      |-|-|-|
                      |`allGiraffeGoHome()`|`allGiraffeComeOut()`|Yes|
                      |`allGiraffeGoHome()`|`drink()`|No |
                      |`allGiraffeGoHome()`|`g.eat()`|Yes|
                      |`eat()`|`allGiraffeComeOut()`|Yes|
                      |`eat()`|`drink()`|Yes|
                      |`eat()`|`g.eat()`|Yes|
-  Do you understand why the following lines fail to compile?
    ```java
    1: public class Gorilla {
    2:   public static int count;
    3:   public static void addGorilla() { count++; }
    4:   public void babyGorilla() { count++; }
    5:   public void announceBabies() {
    6:     addGorilla();
    7:     babyGorilla();
    8:   }
    9:   public static void announceBabiesToEveryone() {
    10:    addGorilla();
    11:    babyGorilla(); // DOES NOT COMPILE
    12:  }
    13:  public int total;
    14:  public static double average
    15:     = total / count; // DOES NOT COMPILE
    16: }
    ```
    - Lines 3 and 4 are fine because both `static` and instance
      methods can refer to a `static` variable. Lines 5–8 are fine
      because an instance method can call a `static` method. Line
      11 doesn’t compile because a `static` method cannot call an
      instance method. Similarly, line 15 doesn’t compile because
      a `static` variable is trying to use an instance variable.
      - A common use for `static` variables is counting the number
      of instances:
        ```java
        public class Counter {
          private static int count;
          public Counter() { count++; }
          public static void main(String[] args) {
            Counter c1 = new Counter();
            Counter c2 = new Counter();
            Counter c3 = new Counter();
            System.out.println(count); // 3
          }
        }
        ```
        - Each time the constructor is called, it increments `count` by
        one. This example relies on the fact that `static` (and
        instance) variables are automatically initialized to the
        default value for that type, which is `0` for `int`. 
        Also notice that we didn’t write `Counter.count`. We could
        have. It isn’t necessary because we are already in that
        class, so the compiler can infer it.
          - **Static Variable Modifiers**
          `static` variables can be declared with the same modifiers as 
          instance variables, such as `final`, `transient`, and `volatile`. 
          While some `static` variables are meant to change as the 
          program runs, like our `count` example, others are meant 
          to never change. This type of `static` variable is known as 
          a _constant_. It uses the `final` modifier to ensure the variable 
          never changes.
            - Constants use the modifier `static final` and a different
            naming convention than other variables. They use all
            uppercase letters with underscores between “words.”
            Example:
              ```java
              public class ZooPen {
                private static final int NUM_BUCKETS = 45;
                public static void main(String[] args) {
                  NUM_BUCKETS = 5; // DOES NOT COMPILE
                }
              }
              ```
              - The compiler will make sure that you do not accidentally
              try to update a `final` variable. Do you think the following 
              compiles?
                ```java
                import java.util.*;
                public class ZooInventoryManager {
                  private static final String[] treats = new String[10];
                  public static void main(String[] args) {
                    treats[0] = "popcorn";
                  }
                }
                ```
                - It actually does compile since treats is a reference variable.
                We are allowed to modify the referenced object or array’s
                contents. All the compiler can do is check that we don’t try
                to reassign `treats` to point to a different object.
                  - The rules for `static final` variables are similar to instance
                  `final` variables,  they use `static` initializers instead of 
                  instance initializers.
                    - ```java
                      public class Panda {
                        final static String name = "Ronda";
                        static final int bamboo;
                        static final double height; // DOES NOT COMPILE
                        static { bamboo = 5;}
                      }
                      ```
                      The `name` variable is assigned a value when it is declared,
                      while the `bamboo` variable is assigned a value in a `static`
                      initializer. The `height` variable is not assigned a value
                      anywhere in the class definition, so that line does not
                      compile. Remember, `final` variables must be initialized with
                      a value.
- **`static` Initializers**
Before, we covered instance initializers that looked like unnamed methods
—just code inside braces. Now we introduce `static` initializers, which look 
similar. We just add the `static` keyword to specify that they should be run 
when the class is first loaded. Here’s an example:
  ```java
  private static final int NUM_SECONDS_PER_MINUTE;
  private static final int NUM_MINUTES_PER_HOUR;
  private static final int NUM_SECONDS_PER_HOUR;
  static {
    NUM_SECONDS_PER_MINUTE = 60;
    NUM_MINUTES_PER_HOUR = 60;
  }
  static {
    NUM_SECONDS_PER_HOUR
        = NUM_SECONDS_PER_MINUTE * NUM_MINUTES_PER_HOUR;
  }
  ```
  - **All `static` initializers run when the class is first used**, in the
  order they are defined. The statements in them run and assign 
  any `static` variables as needed. `final` variables aren’t allowed 
  to be reassigned. The key here is that the static initializer is the 
  first assignment.
    - Let’s try another example to make sure you understand the
distinction:
      ```java
      14: private static int one;
      15: private static final int two;
      16: private static final int three = 3;
      17: private static final int four; // DOES NOT COMPILE
      18: static {
      19:   one = 1;
      20:   two = 2;
      21:   three = 3; // DOES NOT COMPILE
      22:   two = 4;  // DOES NOT COMPILE
      23: }
      ```
      - Line 14 declares a `static` variable that is not `final`. It can be
      assigned as many times as we like. Line 15 declares a `final`
      variable without initializing it. This means we can initialize
      it exactly once in a `static` block. Line 22 doesn’t compile
      because this is the second attempt. Line 16 declares a `final`
      variable and initializes it at the same time. We are not
      allowed to assign it again, so line 21 doesn’t compile. Line
      17 declares a `final` variable that never gets initialized. The
      compiler gives a compiler error because it knows that the
      `static` blocks are the only place the variable could possibly
      be initialized. Since the programmer forgot, this is clearly
      an error.
- **Static Imports**
Before, you saw that you can import a specific class or all
the classes in a package. Example:
  ```java
  import java.util.ArrayList;
  import java.util.*;
  ```
  - There is another type of import called a _static import_. Regular
    imports are for importing classes, while `static` imports are
    for importing `static` members of classes like variables and
    methods.
    Just like regular imports, you can use a wildcard or import
    a specific member. The idea is that you shouldn’t have to
    specify where each `static` method or variable comes from
    each time you use it. 
      - Without `static` import
        ```java
        import java.util.List;
        import java.util.Arrays;
        public class Imports {
          public static void main(String[] args) {
            List<String> list = Arrays.asList("one", "two");
          }
        }
        ```
        With `static` import
        ```java
        import java.util.List;
        import static java.util.Arrays.asList; // static import
        public class ZooParking {
          public static void main(String[] args) {
            List<String> list = 
                      asList("one", "two"); // No Arrays.prefix
          }
        }
        ```
        - In this example, we are specifically importing the `asList`
        method. This means that any time we refer to `asList` in the
        class, it will call `Arrays.asList()`.
        **An interesting case is what would happen if we created 
        an `asList` method in our `ZooParking` class. Java would give 
        it preference over the imported one, and the method we
        coded would be used**.
          - Can you figure out what is wrong with each one?
            ```java
            1: import static java.util.Arrays; // DOES NOT COMPILE
            2: import static java.util.Arrays.asList;
            3: static import java.util.Arrays.*; // DOES NOT COMPILE
            4: public class BadZooParking {
            5:   public static void main(String[] args) {
            6:     Arrays.asList("one");  // DOES NOT COMPILE
            7:   }
            8: }
            ```
            - Line 1 tries to use a `static` import to import a class.
            Remember that `static` imports are only for importing `static`
            members like a method or variable. Regular imports are for
            importing a class. Line 3 tries to see whether you are
            paying attention to the order of keywords. The syntax is
            `import static` and not vice versa. Line 6 is sneaky. The `asList`
            method is imported on line 2. However, the `Arrays` class is
            not imported anywhere. This makes it OK to write
`asList("one")` but not `Arrays.asList("one")`.
              - In Chapter 1, you learned that importing two classes with the
              same name gives a compiler error. This is true of `static`
              imports as well. The compiler will complain if you try to
              explicitly do a `static` import of two methods with the same
              name or two `static` variables with the same name. Here’s
              an example:
                ```java
                import static zoo.A.TYPE;
                import static zoo.B.TYPE; // DOES NOT COMPILE
                ```
                Luckily, when this happens, we can just refer to the `static`
                members via their class name in the code instead of trying
                to use a `static` import. 