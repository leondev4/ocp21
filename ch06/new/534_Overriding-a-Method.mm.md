---

markmap:
  initialExpandLevel: 1
---
# **Overriding a Method**
-  Overriding a method occurs when a subclass declares a new implementation for
  an inherited method with the same signature and compatible return type.
    - When you override a method, you may still reference the parent version of the method 
    using the `super` keyword. In this manner, the keywords `this` and `super` allow you to 
    select between the current and parent versions of a method, respectively. We illustrate 
    this with the following example:
      - ```java
        public class Marsupial {
          public double getAverageWeight() {
            return 50;
          }
        }
        public class Kangaroo extends Marsupial {
          public double getAverageWeight() {
            return super.getAverageWeight()+20;
          }
          public static void main(String[] args) {
            var m = new Marsupial().getAverageWeight();// 50.0
            var n = new Kangaroo().getAverageWeight(); // 70.0
          }
        }
        ```
        - In this example, the `Kangaroo` class overrides the `getAverageWeight()` method 
        but in the process calls the parent version using the `super` reference.
        - **Method Overriding Infinite Calls**
          You might be wondering whether the use of `super` in the previous example was 
          required. For example, what would the following code output if we removed the 
          `super` keyword?
          ```java
          public double getAverageWeight() {
            return getAverageWeight()+20; // StackOverflowError
          }
          ```
          In this example, the compiler would not call the parent `Marsupial` method; it 
          would call the current `Kangaroo` method. The application will attempt to call 
          itself infinitely and produce a `StackOverflowError` at runtime.
          - To override a method, you must follow a number of rules. The compiler performs 
          the following checks when you override a method:
            - 1. The method in the child class must have the same
              signature as the method in the parent class.
              2. The method in the child class must be at least as
              accessible as the method in the parent class.
              3. The method in the child class may not declare a
              checked exception that is new or broader than the class
              of any exception declared in the parent class method.
              4. If the method returns a value, it must be the same or a
              subtype of the method in the parent class, known as
              covariant return types.
- **Rule #1: Method Signatures**
The first rule of overriding a method is somewhat self-explanatory. If two methods 
have the same name but different signatures, the methods are overloaded, not 
overridden. Overloaded methods are not considered  polymorphic as overridden 
methods.
  - **Rule #2: Access Modifiers**
    It must be equal or less restrictive in the child class.
    ```java
    public class Camel {
    public int getNumberOfHumps() {
    return 1;
    } }
    public class BactrianCamel extends Camel {
    private int getNumberOfHumps() { // DOES NOT COMPILE
    }return}
    2;
    ```
    - In this example, `BactrianCamel` attempts to override the `getNumberOfHumps()` 
    method defined in the parent class but fails because the access modifier 
    `private` is more restrictive than the one defined in the parent version of the 
    method.
      - **Rule #3: Checked Exceptions**
        The third rule says that overriding a method cannot declare new checked exceptions or 
        checked exceptions broader than the inherited method.  One implication of this rule is
        that overridden methods are free to declare any number of new unchecked exceptions.
          - Let’s try an example:
            ```java
            public class Reptile {
              protected void sleep() throws IOException {}
              protected void hide() {}
              protected void exitShell() throws FileNotFoundException {}
            }
            public class GalapagosTortoise extends Reptile {
              public void sleep() throws FileNotFoundException {}
              public void hide() throws FileNotFoundException {} // DOES NOT COMPILE
              public void exitShell() throws IOException {} //DOES NOT COMPILE
            }
            ```
            - In this example, we have three overridden methods. These overridden methods use the more accessible 
            `public` modifier, which is allowed per our second rule for overridden methods. The first overridden method
            `sleep()` in `GalapagosTortoise` compiles without issue because the declared exception is narrower than 
            the exception declared in the parent class.
            - The overridden `hide()` method does not compile because it declares a new checked exception not present 
            in the parent declaration. The overridden `exitShell()` also does not compile, since `IOException` is a broader 
            checked exception than `FileNotFoundException`.
- **Rule #4: Covariant Return Types**
The fourth and final rule around overriding a method is probably the most 
complicated, as it requires knowing the relationships between the return 
types. The overriding method must use a return type that is covariant with 
the return type of the inherited method.
  - Let’s try an example for illustrative purposes:
    ```java
    public class Rhino {
      protected CharSequence getName() {
        return "rhino";
      }
      protected String getColor() {
        return "grey, black, or white";
    } }
    public class JavanRhino extends Rhino {
      public String getName() {
        return "javan rhino";
      }
      public CharSequence getColor() { // DOES NOT COMPILE
        return "grey";
    } }
    ```
    - We learned that `String` implements the `CharSequence` interface, making `String` a subtype 
    of `CharSequence`. Therefore, the return type of `getName()` in `JavanRhino` is covariant 
    with the return type of `getName()` in `Rhino`.
    On the other hand, the overridden `getColor()` method does not compile because 
    `CharSequence` is not a subtype of `String`. To put it another way, all `String` values are 
    `CharSequence` values, but not all `CharSequence` values are `String` values. For instance, 
    a `StringBuilder` is a `CharSequence` but not a `String`. For the exam, you need to know if 
    the return type of the overriding method is the same as or a subtype of the return type 
    of the inherited method.
      - **Redeclaring private Methods**
      you can’t override `private` methods since they are not inherited. Just because 
      a child class doesn’t have access to the parent method doesn’t mean the child 
      class can’t define its own version of the method. 
       For example, these two declarations compile:
        ```java
        public class Beetle {
          private String getSize() {
            return "Undefined";
          } }
        public class RhinocerosBeetle extends Beetle {
          private int getSize() {
            return 5;
          } }
        ```
        - Notice that the return type differs in the child method from `String` to `int`. In this example, 
        the method `getSize()` in the parent class is not inherited, so the method in the child class 
        is a new method and not an override of the method in the parent class.
        What if the `getSize()` method was declared `public` in `Beetle`? In this case, the method in
        `RhinocerosBeetle` would be an invalid override. The access modifier in `RhinocerosBeetle` 
        is more restrictive, and the return types are not covariant.
          - **Hiding Static Methods**
            A hidden method occurs when a child class defines a `static` method with the same name and signature as 
            an inherited `static` method defined in a parent class. Method hiding is similar to but not exactly the same 
            as method overriding. The previous four rules for overriding a method must be followed when a method is 
            hidden. In addition, a new fifth rule is added for hiding a method:
            5. The method defined in the child class must be marked as `static` if it is marked as `static` in a parent class.
- Put simply, <strong style="color:green">it is method hiding if the two methods are marked `static` and 
method overriding if they are not marked `static`. If one is marked `static` 
and the other is not, the class will not compile.</strong>
  - Let’s review some examples of the new rule:
      ```java
      public class Bear {
        public static void eat() {
          System.out.println("Bear is eating");
        } }
      public class Panda extends Bear {
        public static void eat() {
          System.out.println("Panda is chewing");
        }
        public static void main(String[] args) {
          eat();
        } }
      ```
      - In this example, the code compiles and runs. The `eat()` method in the `Panda` class 
      hides the `eat()` method in the `Bear` class, printing `"Panda is chewing"` at runtime. 
      Because they are both marked as `static`, this is not considered an overridden method. 
      That said, there is still some inheritance going on. If you remove the `eat()` declaration 
      in the `Panda` class, then the program prints `"Bear is eating"` instead.
        - See if you can figure out why each of the method declarations 
        in the `SunBear` class does not compile:
          ```java
          public class Bear {
            public static void sneeze() {
              System.out.println("Bear is sneezing");
            }
            public void hibernate() {
              System.out.println("Bear is hibernating");
            }
            public static void laugh() {
              System.out.println("Bear is laughing");
            }
          }
          public class SunBear extends Bear {
            public void sneeze() { // DOES NOT COMPILE
              System.out.println("Sun Bear sneezes quietly");
            }
            public static void hibernate() { // DOES NOT COMPILE
              System.out.println("Sun Bear is going to sleep");
            }
            protected static void laugh() { // DOES NOT COMPILE
              System.out.println("Sun Bear is laughing");
            }
          }
          ```
          - In this example, `sneeze()` is marked `static` in the parent class but not in the child class. 
          The compiler detects that you’re trying to override using an instance method. However,
          `sneeze()` is a `static` method that should be hidden, causing the compiler to generate an 
          error. The second method, `hibernate()`, does not compile for the opposite reason. The
          method is marked `static` in the child class but not in the parent class.
          - Finally, the `laugh()` method does not compile. Even though both versions of the method 
          are marked `static`, the version in `SunBear` has a more restrictive access modifier than 
          the one it inherits, and it breaks the second rule for overriding methods. Remember, the 
          four rules for overriding methods must be followed when hiding `static` methods.
            - **Hiding Variables**
              Java doesn’t allow variables to be overridden. Variables can be hidden, though.
              A hidden variable occurs when a child class defines a variable with the same name as an inherited variable
              defined in the parent class. This creates two distinct copies of the variable within an instance of the child 
              class: one instance defined in the parent class and one defined in the child class.
- What do you think the following application
prints?
  ```java
  class Carnivore {
    protected boolean hasFur = false;
  }
  public class Meerkat extends Carnivore {
    protected boolean hasFur = true;
    public static void main(String[] args) {
      Meerkat m = new Meerkat();
      Carnivore c = m;
      System.out.println(m.hasFur); //true
      System.out.println(c.hasFur); //false
    }
  }
  ```
    - Confused about the output? Both of these classes define a `hasFur` variable, but 
    with different values. Even though only one object is created by the `main()` method, 
    both variables exist independently of each other. <strong>The output changes depending 
    on the reference variable used.</strong>
    If you didn’t understand the last example, don’t worry. For now, you just need to 
    know that overriding a method replaces the parent method on all reference variables
    (other than `super`), whereas hiding a method or variable replaces the member 
    only if a child reference type is used.
      - **Writing _`final`_ Methods**
      `final` methods cannot be overridden or hidden.  Let’s take a look at an example:
        ```java
        public class Bird {
          public final boolean hasFeathers() {
            return true;
          }
          public final static void flyAway() {}
        }
        public class Penguin extends Bird {
          public final boolean hasFeathers() { // DOES NOT COMPILE
            return false;
          }
          public final static void flyAway() {} // DOES NOT COMPILE
        }
        ```
        - In this example, the instance method `hasFeathers()` is marked as final in the parent class 
        `Bird`, so the child class `Penguin` cannot override the parent method, resulting in a compiler 
        error. The static method `flyAway()` is also marked `final`, so it cannot be hidden in the 
        subclass. In this example, whether or not the child method uses the `final` keyword is 
        irrelevant; the code will not compile either way.
        This rule applies only to inherited methods. For example, if the two methods were marked 
        `private` in the parent `Bird` class, then the `Penguin` class, as defined, would compile. In
        that case, the private methods would be redeclared, not overridden or hidden.