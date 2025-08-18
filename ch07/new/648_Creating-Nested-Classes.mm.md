---

markmap:
  initialExpandLevel: 1
---
# **Creating Nested Classes**
- A nested class is a class that is defined within another class. A nested class can come 
in one of four flavors, with all supporting instance and static variables as members.
**Inner class:** A non-`static` type defined at the member level of a class
**Static nested class:** A `static` type defined at the member level of a class
**Local class:** A class defined within a method body
**Anonymous class:** A special case of a local class that does not have a name
  - There are many benefits of using nested classes. They can define helper classes and restrict them to the 
  containing class, thereby improving encapsulation. They can make it easy to create a class that will be 
  used in only one place. They can even make the code cleaner and easier to read.
    - **Declaring an Inner Class**
      An inner class, also called a member inner class, is a non-`static` type defined at the member level of a class 
      (the same level as the methods, instance variables, and constructors). Because they are not top-level types, 
      they can use any of the four access levels, not just `public` and package access.
      - Inner classes have the following properties:
        \* Can be declared `public`, `protected`, package, or `private`
        \* Can extend a class and implement interfaces
        \* Can be marked `abstract` or `final`
        \* Can access members of the outer class, including `private` members
        The last property is pretty cool. It means that the inner class can access 
        variables in the outer class without doing anything special. 
          -  Ready for a complicated way to print `Hi` three times?
              ```java
              1: public class Home {
              2:   private String greeting = "Hi"; // Outer class instance variable
              3:
              4:   protected class Room { // Inner class declaration
              5:     public int repeat = 3;
              6:     public void enter() {
              7:       for (int i = 0; i < repeat; i++) greet(greeting);
              8:     }
              9:    private static void greet(String message) {
              10:     System.out.println(message);
              11:   }
              12: }
              13:
              14:   public void enterRoom() { // Instance method in outer class
              15:     var room = new Room(); // Create the inner class instance
              16:     room.enter();
              17: }
              18:   public static void main(String[] args) {
              19:     var home = new Home();  // Create the outer class instance
              20:     home.enterRoom();
              21: } }
              ```
              - An inner class declaration looks just like a stand-alone class declaration except that it happens to be located 
              inside another class. Line 7 shows that the inner class just refers to `greeting` as if it were available in the 
              `Room` class. This works because it is, in fact, available. Even though the variable is `private`, it is accessed 
              within that same class.
              - Since an inner class is not `static`, it has to be called using an instance of the outer class. That means you have 
              to create two objects. Line 19 creates the outer `Home` object, while line 15 creates the inner `Room` object. It’s
              important to notice that line 15 doesn’t require an explicit instance of `Home` because it is an instance method 
              within `Home`. This works because `enterRoom()` is an instance method within the `Home` class. Both `Room` 
              and `enterRoom()` are members of `Home`.
                - **Creating an Instance of an Inner Class**
                There is another way to instantiate `Room` that looks odd at first. OK, well, maybe not 
                just at first.  This syntax isn’t used often enough to get used to it:
                  ```java
                  20:   public static void main(String[] args) {
                  21:     var home = new Home();
                  22:     Room room = home.new Room(); // Create the inner class instance
                  23:     room.enter();
                  24:   }
                  ```
                  - Let’s take a closer look at lines 21 and 22. We need an instance of `Home` to create a `Room`. 
                  We can’t just call `new Room()` inside the `static main()` method, because Java won’t know
                    which instance of `Home` it is associated with. Java solves this by calling `new` as if it were 
                    a method on the `home` variable. We can shorten lines 21–23 to a single line:
                    ```java
                    21:     new Home().new Room().enter(); // Sorry, it looks ugly to us too!
                    ```
- **Referencing Members of an Inner Class**
Inner classes can have the same variable names as outer classes, making scope a little 
tricky. There is a special way of calling `this` to say which variable you want to access.
In fact, you aren’t limited to just one inner class. While the following is common on the 
exam, please never do this in code you write. Here is how to nest multiple classes and
access a variable with the same name in each:
  - ```java
    1: public class A {
    2:   private int x = 10;
    3:   class B {
    4:     private int x = 20;
    5:       class C {
    6:         private int x = 30;
    7:         public void allTheX() {
    8:           System.out.println(x);         // 30
    9:           System.out.println(this.x);    // 30
    10:          System.out.println(B.this.x);  // 20
    11:          System.out.println(A.this.x);  // 10
    12:   } } }
    13:   public static void main(String[] args) {
    14:     A a = new A();
    15:     A.B b = a.new B();
    16:     A.B.C c = b.new C();
    17:     c.allTheX();
    18: }}
    ```
    - Yes, this code makes us cringe too. It has two nested classes. Line 14 instantiates the outermost one. 
    Line 15 uses the awkward syntax to instantiate a `B`. Notice that the type is `A.B`. We could have written
    `B` as the type because that is available at the member level of `A`. Java knows where to look for it. On 
    line 16, we instantiate a `C`. This time, the `A.B.C` type is necessary to specify. `C` is too deep for Java to
    know where to look. Then line 17 calls a method on the instance variable `c`.
    Lines 8 and 9 are the type of code that we are used to seeing. They refer to the instance variable on 
    the current class—the one declared on line 6, to be precise. Line 10 uses `this` in a special way. We still 
    want an instance variable. But this time, we want the one on the `B` class, which is the variable on line 4. 
    Line 11 does the same thing for class `A`, getting the variable from line 2.
      - **Inner Classes Require an Instance**
        Take a look at the following and see whether you can
        figure out why two of the three constructor calls do not
        compile:
        ```java
        public class Fox {
          private class Den {}
          public void goHome() {
            new Den();
          }
          public static void visitFriend() {
            new Den(); // DOES NOT COMPILE
          }
        }

        public class Squirrel {
          public void visitFox() {
            new Den(); // DOES NOT COMPILE
          }
        }
        ```
        - The first constructor call compiles because `goHome()` is an instance method, and therefore the call is associated
        with the this instance. The second call does not compile because it is called inside a `static` method. You can still
        call the constructor, but you have to explicitly give it a reference to a `Fox` instance.
        The last constructor call does not compile for two reasons. Even though it is an instance method, it is not an 
        instance method inside the `Fox` class. Adding a `Fox` reference would not fix the problem entirely, though. `Den`
        is `private` and not accessible in the `Squirrel` class.
          - **Creating a `static` Nested Class**
            A `static` nested class is a `static` type defined at the member level. Unlike an inner class, a `static` nested 
            class can be instantiated without an instance of the enclosing class. **Static nested classes cannot 
            access instance variables and methods declared in the outer class.**
              - In other words, it is like a top-level class except for the following:
              \* The nesting creates a namespace because the enclosing class name must be used to refer to it.
              \* It can additionally be marked `private` or `protected`.
              \* The enclosing class can refer to the fields and methods of the `static` nested class.
                - Let’s take a look at an example:
                  ```java
                  1: public class Park {
                  2:   static class Ride {
                  3:     private int price = 6;
                  4:   }
                  5:   public static void main(String[] args) {
                  6:     var ride = new Ride();
                  7:     System.out.println(ride.price);
                  8:  } }
                  ```
                  Line 6 instantiates the nested class. Since the class is `static`,
                  you do not need an instance of `Park` to use it. You are
                  allowed to access `private` instance variables, as shown on
                  line 7.
- <strong style="color:green">Nested Records are Implicitly `static`</strong>
If you see a nested record, it is implicitly `static`. This means it can be used without a reference
to the outer class. It also means <strong style="color:green">it cannot access instance members of the outer class.</strong> 
We can compare and contrast this with two implementations of `Emu`, one that uses a record
and the other that uses a class.
  - ```java
    11: class Emu1 {
    12:   String name = "Emmy";
    13:   static Feathers createFeathers() {
    14:     return new Feathers("grey");
    15:   }
    16:   record Feathers(String color) {
    17:     void fly() {
    18:       System.out.print(name + " is flying"); // DOES NOT COMPILE
    19:   } } }
    20:
    21: class Emu2 {
    22:   String name = "Emmy";
    23:   static Feathers createFeathers() {
    24:     return new Feathers("grey"); // DOES NOT COMPILE
    25:   }
    26:   class Feathers {
    27:     void fly() {
    28:     System.out.print(name + " is flying");
    29:   } } }
    ```
    - Line 14 compiles without issue because the record is implicitly `static`. Line 24 does not compile, 
    though, as the class version of `Feathers` is not `static` and would require an instance of `Emu2` to 
    create. Likewise, the outer variable, `name`, is only visible to the nested class if it is not `static`, as 
    shown by line 28 compiling and line 18 not compiling.
      - **Writing a Local Class**
        A local class is a nested class defined within a method. Like local variables, a local class declaration 
        does not exist until the method is invoked, and it goes out of scope when the method returns. This 
        means you can create instances only from within the method. Those instances can still be returned 
        from the method. This is just how local variables work. They can be declared inside constructors and 
        initializers
          - Local classes have the following properties:
            \* Do not have an access modifier (like local variables).
            \* The `static` keyword must not be used (like local variables).
            \* Can be declared `final` or `abstract`.
            \* Can include instance and `static` members.
            \* Have access to all fields and methods of the enclosing class (when 
            defined in an instance method).
            \* Can access `final` and effectively final local variables.
            - For an example
              ```java
              1: public class PrintNumbers {
              2:   private int length = 5;
              3:   public void calculate() {
              4:     final int width = 20;
              5:     class Calculator {
              6:       public void multiply() {
              7:         System.out.print(length * width);
              8:       }
              9:     }
              10:    var calculator = new Calculator();
              11:    calculator.multiply();
              12:   }
              13:   public static void main(String[] args) {
              14:     var printer = new PrintNumbers();
              15:     printer.calculate(); // 100
              16:   }
              17: }
              ```
              - Lines 5–9 are the local class. That class’s scope ends on line 12, where the method 
              ends. Line 7 refers  to an instance variable and a final local variable, so both variable 
              references are allowed from within the local class.
              - Earlier, we made the statement that local variable references are 
              allowed if they are `final` or effectively final. As an illustrative example, 
              consider the following:
                ```java
                public void processData() {
                  final int length = 5;
                  int width = 10;
                  int height = 2;
                  class VolumeCalculator {
                    public int multiply() {
                      return length * width * height; // DOES NOT COMPILE
                    }
                  }
                  width = 2;//It is not effectively final
                }
                ```
                - The `length` and `height` variables are `final` and effectively final, respectively, so neither causes
                 a compilation issue. On the other hand, the `width` variable is reassigned during the method, 
                 so it cannot be effectively final. For this reason, the local class declaration does not compile.
- **Defining an Anonymous Class**
An anonymous class is a specialized form of a local class that does not have a name. It is 
declared and instantiated all in one statement using the `new` keyword, a type name with 
parentheses, and a set of braces `{}`. Anonymous classes must extend an existing class or
implement an existing interface. They are useful when you have a short implementation 
that will not be used anywhere else. Here’s an example:
  - ```java
    1: public class ZooGiftShop {
    2:   abstract class SaleTodayOnly {
    3:     abstract int dollarsOff();
    4:  }
    5:  public int admission(int basePrice) {
    6:    SaleTodayOnly sale = new SaleTodayOnly() {
    7:      int dollarsOff() { return 3; }
    8:    }; // Don't forget the semicolon!
    9:    return basePrice - sale.dollarsOff();
    10: } }
    ```
    - Lines 2–4 define an `abstract` class. Lines 6–8 define the anonymous class. Notice how this anonymous 
    class does not have a name. The code says to instantiate a new `SaleTodayOnly` object. But wait: 
    `SaleTodayOnly` is `abstract`. This is OK because we provide the class body right there, anonymously. 
    In this example, writing an anonymous class is equivalent to writing a local class with an unspecified 
    name that extends `SaleTodayOnly` and immediately uses it.
    <strong>Pay special attention to the semicolon on line 8. We are declaring a local variable on these lines.</strong> 
    Local variable declarations are required to end with semicolons, just like other Java statements, even if 
    they are long and happen to contain an anonymous class.
      - Now we convert this same example to implement an interface
       instead of extending an `abstract` class:
        ```java
        1: public class ZooGiftShop {
        2:   interface SaleTodayOnly {
        3:     int dollarsOff();
        4:   }
        5:   public int admission(int basePrice) {
        6:     var sale = new SaleTodayOnly() {
        7:        public int dollarsOff() { return 3; }
        8:     };
        9:     return basePrice - sale.dollarsOff();
        10: } }
        ```
        - The most interesting thing here is how little has changed. Lines 2–4 declare an `interface` 
        instead of an `abstract` class. Line 7 is `public` instead of using default access since 
        interfaces require  `abstract` methods to be `public`. And that is it. The anonymous class is 
        the same whether you implement an interface or extend a class! Java figures out which 
        one you want automatically. Just remember that in this second example, an instance of 
        a class is created on line 6, not an interface.
        But what if we want to both implement an `interface` and extend a class? You can’t do so 
        with an anonymous class unless the class to extend is `java.lang.Object`. The `Object`
        class doesn’t count in the rule.
          - You can even define anonymous classes outside a method body. The following may look 
          like we  are instantiating an interface as an instance variable, but the `{}` after the interface 
          name indicates that this is an anonymous class implementing the interface:
            ```java
            public class Gorilla {
              interface Climb {}
              Climb climbing = new Climb() {};
            }
            ```
            - For the exam, make sure you know the information in Table 7.4 about which syntax 
            rules are permitted in Java. <br/>
              **TABLE 7.4** Modifiers in nested classes
              |Permitted modifiers|Inner class|`static` nested class|Local class|Anonymous class|
              |-|-|-|-|-|
              |Access modifiers|All|All|None|None|
              |`abstract`|Yes|Yes|Yes|No|
              |`final`|Yes|Yes|Yes|No|

              <br/>

              You should also know the information in Table 7.5 about types of access. For example, 
              the exam might try to trick you by having a `static` class access an outer class instance
              variable without a reference to the outer class.
              - **TABLE 7.5** Nested class access rules
                 ||Inner class|`static` nested class|Local class|Anonymous class|
                |-|-|-|-|-|                
                |Can include instance and `static` members?|Yes|Yes|Yes|Yes|                
                |Can extend a class or implement any number<br/> of interfaces?|Yes|Yes|Yes|No, must have exactly<br/> one superclass or one <br/>interface|                
                |Can access instance members of enclosing <br/>class?|Yes|No|Yes (if `final` or<br/> effectively final)|Yes (if `final` or<br/> effectively final)| 
                |Can access local variables of enclosing<br/> method?|N/A|N/A|Yes (if `final` or<br/> effectively final)|Yes (if `final` or<br/> effectively final)|                


