---

markmap:
  initialExpandLevel: 1
---

# **Beyond Classes**
- For this chapter, remember that a Java file may have at most one public top-level type, 
and it must match the name of the file. This applies to classes, enums, records, and so 
on. Also, remember that a top-level type can only be declared with `public` or package 
access.
  - **Annotations to Know for the Exam**
   `@Override`, `@FunctionalInterface`, `@Deprecated`, `@SuppressWarnings` and `@SafeVarargs`
    - **Implementing Interfaces**
      Since classes can only extend one class, they had limited use for inheritance. 
      On the other hand, a class may implement any number of interfaces. An interface 
      is an abstract data type that declares a list of abstract methods that any class 
      implementing the interface must provide.
        - **Declaring and Using an Interface**
        In Java, an interface is defined with the `interface` keyword, analogous to the `class` keyword 
        used when defining a class. Refer to [Figure 7.1](https://1drv.ms/i/c/c83cfca51d5c2032/EXbRTA5UmA1HhxowAViWnMsBKXKM0pvtBQnftetspEXaWA?e=PKTFjA) for a proper interface declaration.
          - In Figure 7.1, our interface declaration includes a single abstract method and a constant 
          variable. **Interface variables** are referred to as constants because they **are assumed to 
          be `public`, `static`, and `final`.** They are initialized with a constant value when they are 
          declared. Since they are `public` and `static`, they can be used outside the interface
          declaration without requiring an instance of the interface. Figure 7.1 also includes an 
          **abstract method** that, like an interface variable, **is assumed to be `public`.**
            - One aspect of an interface declaration that differs from an abstract class is that it contains 
            implicit modifiers. An implicit modifier is a modifier that the compiler automatically inserts
            into the code. For example, an interface is always considered to be `abstract`, even if it is
              not marked so!
- ```java
  public abstract interface WalksOnTwoLegs {}
  ```
  It compiles because interfaces are not required to define any methods. The `abstract` 
  modifier in this example is optional for interfaces, with the compiler inserting it if it is
  not provided. 
    - Consider the following two examples:
        ```java
        final interface WalksOnEightLegs {} // DOES NOT COMPILE
        public class Biped {
          public static void main(String[] args) {
            var e = new WalksOnTwoLegs(); // DOES NOT COMPILE
          }
        }
        ```
        The `WalksOnEightLegs` interface doesn’t compile because interfaces cannot be marked as 
        `final` for the same reason that `abstract` classes cannot be marked as `final`. Marking an
        interface `final` implies no class could ever implement it. The `Biped` class also doesn’t 
        compile, as `WalksOnTwoLegs` is an interface and cannot be instantiated.
        - Let’s say we have an interface `Climb`, defined as follows:
          ```java
          public interface Climb {
            Number getSpeed(int age);
          }
          ```
          Next, we have a concrete class `FieldMouse` that implements the Climb interface, 
          as shown in [Figure 7.2](https://1drv.ms/i/c/c83cfca51d5c2032/ETNgDAtbAAtAizt3XKyCQm0BCuyWf4ygw86gH1o9Byoikw?e=H62hUU).
          - The `FieldMouse` class declares that it implements the `Climb` interface and includes an
          overridden version of `getSpeed()` inherited from the `Climb` interface. The `@Override` 
          annotation is optional. It serves to let other developers know that the method is inherited.
          The method **signature** of `getSpeed()` **matches exactly**, and the **return type is 
          covariant**, since a `Float` can be implicitly cast to a `Number`. The access modifier of 
          the interface method is implicitly `public` in `Climb`, although the concrete class
          `FieldMouse` must explicitly declare it.
            - As shown in Figure 7.2, a class can implement multiple interfaces, each separated by 
            a comma (`,`). If any of the interfaces define abstract methods, then the concrete class
            is required to override them. In this case, `FieldMouse` implements the `CanBurrow` 
            interface that we saw in Figure 7.1. In this manner, the class overrides two abstract
            methods at the same time with one method declaration.
              - **Extending an Interface**
                  Like a class, an interface can extend another interface
                  using the `extends` keyword.
                  ```java
                  public interface Nocturnal {}
                  public interface HasBigEyes extends Nocturnal {}
                  ```
                  - Unlike a class, which can extend only one class, an interface can 
                  extend multiple interfaces.
                    ```java
                    public interface Nocturnal {
                      public int hunt();
                    }
                    public interface CanFly {
                      public void flap();
                    }
                    public interface HasBigEyes extends Nocturnal, CanFly {}
                    public class Owl implements HasBigEyes {
                      public int hunt() { return 5; }
                      public void flap() { System.out.println("Flap!"); }
                    }
                    ```
                    -  The `Owl` class implements the `HasBigEyes` interface and must implement the `hunt()` and 
                    `flap()` methods. Extending two interfaces is permitted because interfaces are not initialized 
                    as part of a class hierarchy. Unlike abstract classes, they do not contain constructors and 
                    are not part of instance initialization. Interfaces simply define a set of rules and methods 
                    that a class implementing them must follow.
- **Inheriting an Interface**
  Like an abstract class, when a concrete class implements an interface, all of the inherited 
  abstract methods must be implemented. How many abstract methods does the concrete
  `Swan` class inherit?
  - ```java
    abstract class Animal{  abstract int getType();  }

    interface Fly{  void fly(); }

    interface Swim{  void swim();  }

    abstract class Bird implements Fly{
      abstract boolean canSwoop();
    }
    public class Swan extends Bird implements Swim{
      //???
    }
    ```
    - The concrete `Swan` class inherits four abstract methods that it must override: 
    `getType()`, `canSwoop()`, `fly()`, and `swim()`.
      Let’s take a look at another example involving an abstract class that implements 
      an interface:
        ```java
        public interface HasTail {  public int getTailLength();  }

        public interface HasWhiskers {  public int getNumberOfWhiskers();  }

        public abstract class HarborSeal implements HasTail, HasWhiskers {}

        public class CommonSeal extends HarborSeal {}// DOES NOT COMPILE
        ```
        - The `HarborSeal` class compiles because it is `abstract` and not required to implement 
        any of the `abstract` methods it inherits. The concrete `CommonSeal` class, though, 
        must override all inherited abstract methods.
          - **Mixing Class and Interface Keywords**
          Although a class can implement an interface, a class cannot extend an interface. Likewise,
          while an interface can extend another interface, an interface cannot implement another 
          interface or inherit a class. The following examples illustrate these principles:
            ```java
            public interface CanRun {}
            public class Cheetah extends CanRun {} // DOES NOT COMPILE
            public class Hyena {}
            public interface HasFur extends Hyena {} // DOES NOT COMPILE
            ```
            - **Inheriting Duplicate Abstract Methods**
              Java supports inheriting two abstract methods that have compatible method declarations.
                ```java
                public interface Herbivore { public int eatPlants(int plantsLeft); }
                
                public interface Omnivore { public int eatPlants(int foodRemaining); }

                public class Bear implements Herbivore, Omnivore {
                  public int eatPlants(int plants) {
                    System.out.print("Eating plants");
                    return plants - 1;
                  } }
                ```
                - By compatible, we mean a method can be written that properly overrides both inherited 
                methods: for example, by using covariant return types. Notice that the method parameter 
                names don’t need to match, just the type.
                The following is an example of an incompatible declaration:
                  ```java
                  public interface Herbivore { public void eatPlants(int plantsLeft); }

                  public interface Omnivore { public int eatPlants(int foodRemaining); }

                  public class Tiger implements Herbivore, Omnivore { // DOES NOT COMPILE
                    // Doesn't matter!
                  }
                  ```
                  - The implementation of `Tiger` doesn’t matter in this case since it’s impossible to write a 
                  version of `Tiger` that satisfies both inherited `abstract` methods. The code does not
                  compile, regardless of what is declared inside the `Tiger` class.
- **Inserting Implicit Modifiers**
As mentioned earlier, an implicit modifier is one that the compiler will automatically insert. 
It’s reminiscent of the compiler inserting a default no-argument constructor if you do not 
define a constructor. You can choose to insert  these implicit modifiers yourself or let the 
compiler insert them for you.
  - The following list includes the implicit modifiers for  interfaces that you 
  need to know for the exam:
    \* Interfaces are implicitly `abstract`.
    \* Interface variables are implicitly `public`, `static`, and `final`.
    \* Interface methods without a body are implicitly `abstract`.
    \* Interface methods without the `private` modifier are implicitly `public`.
    - The last rule applies to `abstract`, `default`, and `static` interface methods.
    Let’s take a look at an example. The following two interface definitions 
    are equivalent, as the compiler will convert them both to the second 
    declaration:
      - ```java
        public interface Soar {
          int MAX_HEIGHT = 10;
          final static boolean UNDERWATER = true;
          void fly(int speed);
          abstract void takeoff();
          public abstract double dive();
        }
        public abstract interface Soar {
          public static final int MAX_HEIGHT = 10;
          public final static boolean UNDERWATER = true;
          public abstract void fly(int speed);
          public abstract void takeoff();
          public abstract double dive();
        }
        ```
        - **Conflicting Modifiers**
          If an abstract method is implicitly `public`, can it be explicitly marked `protected` or 
          `private`?
          ```java
          public interface Dance {
            private int count = 4; // DOES NOT COMPILE
            protected void step(); // DOES NOT COMPILE
          }
          ```
          Neither of these interface member declarations compiles, as the compiler will 
          apply the `public` modifier to both, resulting in a conflict.
          - **Differences between Interfaces and Abstract Classes**
            Even though abstract classes and interfaces are both considered abstract types, 
            only **interfaces make use of implicit modifiers.** How do the `play()` methods 
            differ in the following two definitions?
            ```java
            abstract class Husky {  // abstract modifier required
              abstract void play(); // abstract modifier required
            }
            interface Poodle {      // abstract modifier optional
              void play();          // abstract modifier optional
            }
            ```
            - Both of these method definitions are considered abstract. That said, the `Husky` class will 
            not compile if the `play()` method is not marked `abstract`, whereas the method in the
            `Poodle` interface will compile with or without the `abstract` modifier.
              - What about the access level of the `play()` method? 
                ```java
                public class Webby extends Husky {
                  void play() {} // play() is declared with package access in Husky
                }
                public class Georgette implements Poodle {
                  void play() {} // DOES NOT COMPILE - play() is public in Poodle
                }
                ```
                The `Webby` class compiles, but the `Georgette` class does not. Even though the
                two method implementations are identical, the method in the `Georgette` class 
                reduces the access modifier on the method from `public` to package access.
- **Declaring Concrete Interface Methods**
In **Table 7.1**, the membership type determines how it is able to be accessed. A method 
with a membership type of class is shared among all instances of the interface, whereas 
a method with a membership type of instance is associated with a particular instance of 
the interface.
  - **TABLE 7.1** Interface member types
    ||Membership<br/>type|Required<br/>modifiers|Implicit modifiers|Has value or body?|
    |-|-|-|-|-|
    |Constant variable|Class|-|`public`, `static`, `final`|Yes|
    |`abstract` method|Instance|-|`public`, `abstract`|No|
    |`default` method|Instance|`default`|`public`|Yes|
    |`static` method|Class|`static`|`public`|Yes|
    |`private` method|Instance|`private`|-|Yes|
    |`private static` method|Class|`private static`|-|Yes|
    - **What About protected or Package Interface Members?**
    Interfaces do not support protected members. They also do 
    not support package access members.
      - **Writing a default Interface Method**
      A default method is a method defined in an interface with the `default` keyword and includes
      a method body. It may be optionally overridden by a class implementing the interface.
      One use of `default` methods is for backward compatibility. You can add a new `default` 
      method to an interface without the need to modify all of the existing classes that implement 
      the interface. The older classes will just use the default implementation of the method defined 
      in the interface.
        - The following is an example of a default method defined in an interface:
          ```java
          public interface IsColdBlooded {
            boolean hasScales();
            public default double getTemperature(){//public is optional
              return 10.0;
            }
          }
          ```
          - This example defines two interface methods, one `abstract` and one `default`. The following 
          `Snake` class, which implements `IsColdBlooded`, must implement `hasScales()`. It may 
          rely on the default implementation of `getTemperature()` or override the method with its 
          own version:
            ```java
            public class Snake implements IsColdBlooded {
              public boolean hasScales() { // Required override
                return true;
              }
              public double getTemperature() { // Optional override
                return 12.2;
              } }
            ```
            - **Default Interface Method Definition Rules**
              1.- A `default` method may be declared only within an interface.
              2.- A `default` method must be marked with the `default` keyword and include a method body.
              3.- A `default` method is implicitly `public`.
              4.- A `default` method cannot be marked `abstract`, `final`, or `static`.
              5.- A `default` method may be overridden by a class that implements the interface.
              6.- If a class inherits two or more `default` methods with the same method signature, 
              &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;then the class must override the method.
              - The first rule should give you some comfort in that you’ll only see `default` methods 
              in interfaces. If you see them in a class or enum on the exam, something is wrong. 
              The second rule just denotes syntax, as `default` methods must use the `default`
              keyword. For example, the following code snippets will not compile because they 
              mix up concrete and abstract interface methods:
                ```java
                public interface Carnivore {
                  public default void eatMeat(); // DOES NOT COMPILE
                  public int getRequiredFoodAmount() { // DOES NOT COMPILE
                    return 13;
                  } }
                ```
- The next three rules for `default` methods follow from the relationship with abstract 
interface methods. Like abstract interface methods, `default` methods are implicitly 
`public`. Unlike abstract methods, though, `default` interface methods cannot be 
marked `abstract` since they provide a body. They also cannot be marked as `final`, 
because they are designed so that they can be overridden in classes implementing 
the interface, just like abstract methods. Finally, they cannot be marked `static` since 
they are associated with the instance of the class implementing the interface.
  - **Inheriting Duplicate _`default`_ Methods**
    The last rule for creating a default interface method requires some explanation.
    ```java
    public interface Walk {
      public default int getSpeed() { return 5; }
    }
    public interface Run {
      public default int getSpeed() { return 10; }
    }
    public class Cat implements Walk, Run {} // DOES NOT COMPILE
    ```
    - In this example, `Cat` inherits the two default methods for `getSpeed()`, so which does it use? 
    Since `Walk` and `Run` are considered siblings in terms of how they are used in the `Cat`
    class, it is not clear whether the code should output `5` or `10`; then the `Cat` does not compile. 
    <strong style="color:green">If one of the two interfaces was a class, then the class implementation wins.</strong>
    If the class implementing the interfaces overrides the duplicate default method, the code will 
    compile without issue. By overriding the conflicting method, the ambiguity about which version 
    of the method to call has been removed. For example, the following modified implementation 
    of  `Cat` will compile:
      ```java
      public class Cat implements Walk, Run {
        public int getSpeed() { return 1; }
      }
      ```
      - **Calling a _`default`_ Method**
        A `default` method exists on any object inheriting the interface, not on the interface itself. In other
        words, you should treat it like an inherited method that can be optionally overridden, rather than 
        as a `static` method. Consider the following:
        ```java
        public interface Dance {
          default int getRhythm() { return 33; }
        }
        public class Snake implements Dance {
          static void move() {
            var snake = new Snake();
            System.out.print(snake.getRhythm());
            System.out.print(Dance.getRhythm()); // DOES NOT COMPILE
          } }
        ```
        - The first call to `getRhythm()` compiles because it is called on an instance of the `Snake` class. 
        The second does not compile because it is not a `static` method and requires an instance
        of `Dance`.
        In the previous section, we showed how our `Cat` class could override a pair of conflicting `default`
        methods, but what if the `Cat` class wanted to access the “hidden” version of `getSpeed()` in `Walk` 
        or `Run`? Is it still accessible? Yes, but it requires some special syntax.
          ```java
          public class Cat implements Walk, Run {
            public int getSpeed() {
              return 1;
            }
            public int getWalkSpeed() {
              return Walk.super.getSpeed();
            } }
          ```
          - This is an area where a `default` method `getSpeed()` exhibits properties of both an 
          instance and `static` method. We use the interface name to indicate which method 
          we want to call, but we use the `super` keyword to show that we are following
          instance inheritance, not class inheritance. 
            - **Declaring static Interface Methods**
              Interfaces can also include `static` methods. These methods
              are defined explicitly with the `static` keyword and, for the
              most part, behave just like `static` methods defined in classes.
                - **Static Interface Method Definition Rules**
                1.- A `static` method must be marked with the `static` keyword and include a method body.
                2.- A `static` method without an access modifier is implicitly `public`.
                3.- A `static` method cannot be marked `abstract` or `final`.
                4.- A `static` method is not inherited and cannot be accessed in a class implementing the 
                &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;interface without a reference to the interface name.
- These rules should follow from what you know so far of classes, interfaces, and `static` 
methods. For example, you can’t declare `static` methods without a body in classes,
either. Like `default` and `abstract` interface methods, `static` interface methods are 
implicitly `public` if they are declared without an access modifier. As you see shortly, 
you can use the `private` access modifier with `static` methods.
Let’s take a look at a `static` interface method:
  ```java
  public interface Hop {
    static int getJumpHeight() {//implicit public
      return 8;
    } }
  ```
  - Since the method is defined without an access modifier, the compiler will automatically 
  insert the `public` access modifier. The method `getJumpHeight()` works just like a `static`
  method as defined in a class. In other words, it can be accessed without an instance of 
  a class.
    ```java
    public class Skip implements Hop {
      public int skip() {
        return Hop.getJumpHeight();
      } }
    ```
    - The last rule about inheritance might be a little confusing, so let’s look at an example. The 
    following is an example of a class `Bunny` that implements `Hop` and does not compile:
      ```java
      public class Bunny implements Hop {
        public void printDetails() {
          System.out.println(getJumpHeight()); // DOES NOT COMPILE
        } }
      ```
      Without an explicit reference to the name of the interface, the code will not compile, even 
      though `Bunny` implements `Hop`. This can be easily fixed by using the interface name:
      ```java
          System.out.println(Hop.getJumpHeight());
      ```
      - Notice we don’t have the same problem we did when we inherited two `default` interface 
      methods with the same signature. Java “solved” the multiple inheritance problem of 
      `static` interface methods by not allowing them to be inherited!
        - **Reusing Code with private Interface Methods**
          The last two types of methods that can be added to interfaces are `private` (concrete) and 
          `private static` interface methods. Because both types of methods are `private`, they can 
          only be used in the interface declaration in which they are declared. For this reason, they 
          were added primarily to reduce code duplication. For example, consider the following code 
          sample:
            - ```java
              public interface Schedule {
                default void wakeUp() { checkTime(7); }
                private void haveBreakfast() { checkTime(9); }
                static void workOut() { checkTime(18); }
                private static void checkTime(int hour) {
                  if (hour > 17) {
                    System.out.println("You’re late!");
                  } else {
                    System.out.println("You have "+(17-hour)+" hours left "
                          + "to make the appointment");
                  }
                }
              }
              ```
              - You could write this interface without using a `private` method by copying the contents of the 
              `checkTime()` method into the places it is used. It’s a lot shorter and easier to read if you don’t! 
              Since the authors of Java were nice enough to add this feature for our convenience, we might
              as well use it!
              The difference between a non-`static private` method and a `static` one is analogous to the
              difference between an instance and `static` method declared within a class.
                - **Private Interface Method Definition Rules**
                  1.- A `private` interface method must be marked with the `private` modifier and include a method body.
                  2.- A `private static` interface method may be called by any method within the interface definition.
                  3.- A `private` interface method may only be called by default and other `private` non-`static` methods 
                  within the interface definition.
                - Another way to think of it is that a `private` interface method is only accessible to non-`static` methods 
                defined within the interface. A `private static` interface method, on the other hand, can be accessed 
                by any method in the interface. For both types of `private` methods, a class inheriting the interface 
                cannot directly invoke them.
- **Reviewing Interface Members**
We conclude our discussion of interface members with
Table 7.2, which shows the access rules for members
within and outside an interface.
  - **TABLE 7.2** Interface member access
    |                       | Accessible <br/>from `default`<br/> and `private` <br/> methods <br/>within  the<br/> interface? | Accessible<br/> from `static`<br/> methods<br/> within the <br/>interface? | Accessible<br/> from methods <br/>in classes<br/> inheriting <br/>the interface? | Accessible<br/> without an <br/>instance of<br/> the interface? |
    |-|-|-|-|-|
    |Constant variable|Yes|Yes|Yes|Yes|
    |`abstract` method|Yes|No|Yes|No|
    |`default` method|Yes|No|Yes|No|
    |`static` method|Yes|Yes|Yes (interface<br/> name required)|Yes (interface<br/> name required)
    |`private` method|Yes|No|No|No|
    |`private static` method|Yes|Yes|No|No|
    - While **Table 7.2** might seem like a lot to remember, here are some quick tips for the exam:
    \* Treat `abstract`, `default`, and non-`static private` methods as belonging to an instance of the interface.
    \* Treat `static` methods and variables as belonging to the interface class object.
    \* All `private` interface method types are only accessible within the interface declaration.
      - Using these rules, which of the following methods do not compile?
        ```java
        public interface ZooTrainTour {
          abstract int getTrainName();
          private static void ride() {}
          default void playHorn() { getTrainName(); ride(); }
          public static void slowDown() { playHorn(); }
          static void speedUp() { ride(); }
        }
        ```
        - The `ride()` method is `private` and `static`, so it can be accessed by any `default` or `static` 
        method within the interface declaration. The `getTrainName()` is `abstract`, so it can be 
        accessed by a `default` method associated with the instance. The `slowDown()` method 
        is `static`, though, and cannot call a `default` or `private` method, such as `playHorn()`,
        without an explicit reference object. Therefore, the `slowDown()` method does not compile.