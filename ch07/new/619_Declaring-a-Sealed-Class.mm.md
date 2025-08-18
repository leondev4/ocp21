---

markmap:
  initialExpandLevel: 1
---
# **Declaring a Sealed Class**
-  A sealed class is a class that restricts which other classes may extend it.
    - **Declaring a Sealed Class**
    Let’s start with a simple example. A sealed class declares a list of classes that can extend it, 
    while the subclasses declare that they extend the sealed class. [Figure 7.5](https://1drv.ms/i/c/c83cfca51d5c2032/EfouaAH3p1ZJuqwlhKaQpHQBan3p8JkHhPL0CYRGwvlUvw?e=Z5Q5I2) declares a sealed 
    class with two subclasses.
      - **Sealed Class Keywords**
      **`sealed`**: Indicates that a class or interface may only be extended/implemented by named classes or interfaces
      **`permits`**: Used with the `sealed` keyword to list the classes and interfaces allowed
      **`non-sealed`**: Applied to a class or interface that extends a sealed class, indicating that it can be extended by
      unspecified classes
        - The exam is just as likely to test you on what sealed classes cannot be used for. For
        example, can you see why each of these two sets of declarations do not compile?
          ```java
          public class sealed Frog permits GlassFrog {} // DOES NOT COMPILE
          public final class GlassFrog extends Frog {}
          public abstract sealed class Mammal permits Wolf {}
          public final class Wolf extends Mammal {}
          public final class Tiger extends Mammal {} // DOES NOT COMPILE
          ```
          - The first example does not compile because the `class` and `sealed` modifiers are in the wrong 
          order. The modifier has to be before the `class` type. The second example does not compile 
          because `Tiger` isn’t listed in the declaration of `Mammal`. 
          Declaring a sealed class with the `sealed` modifier is the easy part. Most of the time, if you 
          see a question on the exam about sealed classes, they are testing your knowledge of whether
           the subclass extends the sealed class properly.
            - **Compiling Sealed Classes**
              Let’s say we create a `Penguin` class and compile it in a new package without any other source code. 
              With that in mind, does the following compile?
              ```java
              // Penguin.java
              package zoo;
              public sealed class Penguin permits Emperor {}
              ```
              No, it does not! Why? The answer is that a <strong style="color:green">sealed class needs to be declared (and compiled) 
              in the same package as its direct subclasses.</strong>
                - But what about the subclasses themselves? They must each extend the sealed class. For
                  example, the following two declarations do not compile:
                  ```java
                  // Penguin.java
                  package zoo;
                  public sealed class Penguin permits Emperor {} // DOES NOT COMPILE
                  // Emperor.java
                  package zoo;
                  public final class Emperor {}
                  ```
                  Even though the `Emperor` class is declared, it does not extend the `Penguin` class.
- **Specifying the Subclass Modifier**
<strong style="color:green">Every class that directly extends a sealed class must specify exactly 
one of the following three modifiers: `final`, `sealed`, or `non-sealed`.</strong>
  - **Creating `final` Subclasses**
  The first modifier we’re going to look at that can be applied to a direct subclass of a sealed 
  class is the `final` modifier. A sealed class with only `final` subclasses has a fixed set of types.
    ```java
    public sealed class Antelope permits Gazelle {}
    public final class Gazelle extends Antelope {}
    public class DamaGazelle extends Gazelle {} // DOES NOT COMPILE
    ```
    Just as with a regular class, the `final` modifier prevents the subclass `Gazelle` from being 
    extended further.
      - **Creating sealed Subclasses**
        Next, let’s look at an example using the `sealed` modifier:
        ```java
        public sealed class Fish permits ClownFish {}
        public sealed class ClownFish extends Fish permits OrangeClownFish {}
        public final class OrangeClownFish extends ClownFish {}
        ```
        - The `sealed` modifier applied to the subclass `ClownFish` means the same kind of rules 
        that we applied to the parent class `Fish` must be present. Namely, `ClownFish` defines 
        its own list of permitted subclasses. Notice in this example that `OrangeClownFish` 
        is an indirect subclass of `Fish` but is not named in the `Fish` class.
        Despite allowing indirect subclasses not named in `Fish`, the list of classes that can 
        inherit `Fish` is still fixed at compile time. If you have a reference to a `Fish` object, it 
        must be a `Fish`, `ClownFish`, or `OrangeClownFish`.
          - **Creating `non-sealed` Subclasses**
            The `non-sealed` modifier is used to open a sealed parent class to potentially unknown subclasses.
            ```java
            abstract sealed class Mammal permits Feline {}
            non-sealed class Feline extends Mammal {}
            class Tiger extends Feline {}
            ```
            In this example, we are able to create an indirect subclass of `Mammal`, called `Tiger`, not named
            in the declaration of `Mammal`. Also notice that `Tiger` is not final, so it may be extended by any 
            subclass, such as `BengalTiger`. You can not mix `final` and `non-sealed` modifiers.
            ```java
            class BengalTiger extends Tiger {}
            ```
            - At first glance, this might seem a bit counterintuitive. After all, we were able to create subclasses 
            of `Mammal` that were not declared in `Mammal`. So is `Mammal` still sealed? Yes, but that’s thanks 
            to polymorphism. Any instance of `Tiger` or `BengalTiger` is also an instance of `Feline`,  which
             is named in the `Mammal` declaration. You just need to understand that `Mammal` is sealed to 
             `Feline` and its subclasses.
- **Omitting the `permits` Clause (only `permits`)**
Up until now, all of the examples you’ve seen have required a `permits`
clause when declaring a sealed class, but this is not always the case.
Imagine that you have a `Snake.java` file with two top-level classes
defined inside it:
  ```java
  // Snake.java
  public sealed class Snake permits Cobra {}
  final class Cobra extends Snake {}
  ```
  - In this case, the `permits` clause is optional and can be omitted. The `extends` and `final`, 
  `non-sealed` or `sealed` keywords are still  required in the subclass, though:
    ```java
    // Snake.java
    public sealed class Snake {}
    final class Cobra extends Snake {}
    ```
    - <strong style="color:green">If these classes  were in separate files, this code would not compile! To omit the `permits` 
    clause, the declarations must be in the same file.
      The `permits` clause can also be omitted if the subclasses are nested. </strong>
      ```java
      public sealed class Snake {
        final class Cobra extends Snake {}
      }
      ```
      - We cover nested classes shortly. For now, you just need to know that a nested class is a 
      class defined inside another class and that the omit rule also applies to nested classes.
      **Table 7.3** is a handy reference to these cases.
        - **TABLE 7.3** Usage of the `permits` clause in sealed classes
          |Location of direct subclasses|`permits` clause|
          |-|-|
          |In a different file from the sealed class|Required|
          |In the same file as the sealed class|Optional|
          |Nested inside of the sealed class|Optional|
          - **Referencing Nested Subclasses**
            While it makes the code easier to read if you omit the `permits` clause for nested subclasses, 
            you are welcome to name them. However, the syntax might be different than you expect.
            ```java
            public sealed class Snake permits Cobra { // DOES NOT COMPILE
              final class Cobra extends Snake {}
            }
            ```
            - This code does not compile because `Cobra` requires a reference to the `Snake` namespace. 
              The following fixes this issue:
              ```java
              public sealed class Snake permits Snake.Cobra {
                final class Cobra extends Snake {}
              }
              ```
              When all of your subclasses are nested, we strongly recommend omitting the `permits` class.
- **Sealing Interfaces**
Besides classes, interfaces can also be sealed. The idea is analogous 
to classes, and many of the same rules apply. For example, the sealed
interface must appear in the same package or named module as the 
classes or interfaces that directly extend or implement it.
  - One distinct feature of a sealed interface is that the `permits` list can apply to a class 
  that implements the interface or an interface that extends the interface.
    ```java
    // Sealed interface
    public sealed interface Swims permits Duck, Swan, Floats {}
    // Classes permitted to implement sealed interface
    public final class Duck implements Swims {}
    public non-sealed class Swan implements Swims {}
    // Interface permitted to extend sealed interface
    public non-sealed interface Floats extends Swims {}//non-sealed|sealed
    ```
    - What modifiers are permitted for interfaces that extend a sealed interface? Well, remember 
    that <strong style="color:green">interfaces are implicitly `abstract` and cannot be marked `final`</strong>. For this reason, 
    interfaces that extend a sealed interface can only be marked `sealed` or `non-sealed`. They 
    cannot be marked `final`.
      - **Applying Pattern Matching to a Sealed Class**
        `switch` now supports pattern matching. Imagine if we could treat a sealed class like an
        enum in a `switch` by applying pattern matching. Well, we can! Given a sealed class 
        `Fish` with two direct subclasses:
        ```java
        abstract sealed class Fish permits Trout, Bass {}
        final class Trout extends Fish {}
        final class Bass extends Fish {}
        ```
        - We can define a `switch` expression that does not require a
          `default` clause:
          ```java
          public String getType(Fish fish) {
            return switch (fish) {
              case Trout t -> "Trout!";
              case Bass b -> "Bass!";
            };
          }
          ```
          - This only works because `Fish` is `abstract` and `sealed`, and all possible subclasses are handled. 
          If we remove the `abstract` modifier in the Fish declaration, then the `switch` expression would not 
          compile. 
          If the `Fish` class was not `abstract` or one of the `Trout` or `Bass` classes was not `sealed`, the 
          `default` declaration would be mandatory.
          Like enums, make sure that if a `switch` uses a sealed class with pattern matching that all possible 
          types are covered or a `default` clause is included.
            - **Sealed Class Rules**
              - Sealed classes are declared with the `sealed` and `permits` modifiers.
              - Sealed classes must be declared in the same package or named module as their direct subclasses.
              - Direct subclasses of sealed classes must be marked `final`, `sealed`, or `non-sealed`. For interfaces 
              that extend a sealed interface, only `sealed` and `non-sealed` modifiers are permitted.
              - The `permits` clause is optional if the sealed class and its direct subclasses are declared within the 
              same file or the subclasses are nested within the sealed class.
              - Interfaces can be sealed to limit the classes that implement them or the interfaces that extend them.