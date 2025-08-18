---

markmap:
  initialExpandLevel: 1
---
# **Understanding Inheritance**
-  Inheritance is the process by which a subclass automatically includes certain
members of the class, including primitives, objects, or methods, defined in the 
parent class.
    - We refer to any class that inherits from another class    as a subclass or child class, 
    as it is considered a descendant of that class. Alternatively, we refer to the class 
    that the child inherits from as the superclass or parent class, as it is considered 
    an ancestor of the class.
      - **Declaring a Subclass**
      Let’s begin with the declaration of a class and its subclass. [Figure 6.1](https://1drv.ms/i/c/c83cfca51d5c2032/EXAi102qi5xHi0pqz4pXDbwB6qTgBVJk9KehjLsuNX9GLA?e=hPXbH8) shows 
      an example of a superclass, `Mammal`, and subclass `Rhinoceros`.
        - We indicate a class is a subclass by declaring it with the `extends` keyword. 
        We don’t need to declare anything in the superclass other than making sure 
        it is not marked `final`.
          - One key aspect of inheritance is that it is transitive. Given
          three classes [X, Y, Z], if X extends Y, and Y extends Z, then
          X is considered a subclass or descendant of Z. Likewise, Z
          is a superclass or ancestor of X. We sometimes use the
          term direct subclass or descendant to indicate the class
          directly extends the parent class. For example, X is a direct
          descendant only of class Y, not Z.
            - When one class inherits from a parent class, all `public` and
            `protected` members are automatically available as part of the
            child class. If the two classes are in the same package, then
            package members are available to the child class. `private`
            members are restricted to the class they are defined in and
            are never available via inheritance. 
- Let’s take a look at a simple example:
  ```java
  public class BigCat {
    protected double size;
  }
  public class Jaguar extends BigCat {
    public Jaguar() { size = 10.2; }
    public void printDetails() {
      System.out.print(size);
    }
  }
  public class Spider {
    public void setSize() {
      var nSize = size; // DOES NOT COMPILE
    }
  }
  ```
  - `Jaguar` is a subclass or child of `BigCat`, making `BigCat` a
  superclass or parent of `Jaguar`. In the `Jaguar` class, `size` is
  accessible because it is marked `protected`. Via inheritance,
  the `Jaguar` subclass can read or write `size` as if it were its
  own member. Contrast this with the `Spider` class, which has
  no access to `size` since it is not inherited.
    - **Class Modifiers**
      Like methods and variables, a class declaration can have
      various modifiers. **Table 6.1** lists the modifiers you should
      know for the exam.
      - **TABLE 6.1** Class modifiers
        |Modifier|Description|
        |-|-|
        |`final`|The class may not be extended.|
        |`abstract`|The class is abstract, may contain `abstract` methods, and requires a<br/> concrete subclass to instantiate.|
        |`sealed`|The class may only be extended by a specific list of classes.|
        `non-sealed`|A subclass of a sealed class permits potentially unnamed subclasses.|
        |`static`|Used for `static` nested classes defined within a class.|
        - The `final` modifier prevents a class from being extended any further. For 
        example, the following does not compile:
          ```java
          public class Mammal {}

          public final class Rhinoceros extends Mammal {}

          public class Clara extends Rhinoceros {} // DOES NOT COMPILE
          ```
          On the exam, pay attention to any class marked `final`. If you see another 
          class extending it, you know immediately the code does not compile.
            - **Single Inheritance**
              Java supports single inheritance, by which a class may
              inherit from only one direct parent class. Java also supports
              multiple levels of inheritance, by which one class may
              extend another class, which in turn extends another class.
              You can have any number of levels of inheritance, allowing
              each descendant to gain access to its ancestor’s members.
                - By design, Java doesn’t support multiple inheritance in the language because
                multiple inheritance can lead to complex, often difficult-to-maintain data models. 
                Java does allow one exception to the single inheritance rule, a class may 
                implement multiple interfaces.
- **Inheriting Object**
In Java, all classes inherit from a single class: `java.lang.Object`, or `Object` for 
short. Furthermore, `Object` is the only class that doesn’t have a parent class.
  - The compiler makes improvements for you. For example, 
  the following two are equivalent:
    ```java
    public class Zoo { }

    public class Zoo extends java.lang.Object { }
    ```
    - The key is that when Java sees you define a class that
      doesn’t extend another class, the compiler automatically
      adds the syntax `extends java.lang.Object` to the class
      definition. The result is that every class gains access to any
      accessible methods in the `Object` class. For example, the
      `toString()` and `equals()` methods are available in Object;
      therefore, they are accessible in all classes.
        - When you define a new class that extends an existing class, 
        Java does not automatically extend the `Object` class. Since 
        all classes inherit from `Object`, extending an existing class 
        means the child already inherits from `Object` by definition. 
          - Primitive types such as `int` and `boolean` do not inherit from
          `Object`, since they are not classes. As you learned before, 
          through autoboxing they can be assigned or passed as an
          instance of an associated wrapper class, which does inherit
          `Object`.