---

markmap:
  initialExpandLevel: 1
---
# **Creating Classes**
- **Extending a Class**
Let’s create two files in the same package, `Animal.java` and
`Lion.java`.
  - ```java
    // Animal.java
    public class Animal {
      private int age;
      protected String name;
        public int getAge() {
          return age;
      }
      public void setAge(int newAge) {
        age = newAge;
      }
    }
    ```
    - ```
      // Lion.java
      public class Lion extends Animal {
        protected void setProperties(int age, String n) {
          setAge(age);
          name = n;
        }
        public void roar() {
          var msg = name + ", age " + getAge() + ", says: Roar!";
          System.out.print(msg);
        }
        public static void main(String[] args) {
          var lion = new Lion();
          lion.setProperties(3, "kion");
          lion.roar();
        }
      }
      ```
      - The `age` variable exists in the parent `Animal` class and is not directly
      accessible in the `Lion` child class. It is indirectly accessible via the 
      `setAge()` method. The `name` variable is `protected`, so it is inherited 
      in the `Lion` class and directly accessible. We create the `Lion` instance 
      in the `main()` method and use `setProperties()` to set instance variables. 
      Finally, we call the `roar()` method, which prints the following:
        ```java
        kion, age 3, says: Roar!
        ```
        - Let’s take a look at the members of the `Lion` class. The instance 
        variable `age` is marked `private` and is not directly accessible from 
        the subclass `Lion`. Therefore, the following would not compile:
          ```java
          public class Lion extends Animal {
            public void roar() {
              var msg = "Lion's age: " + age; //DOES NOT COMPILE
              System.out.print(msg);
            }
          }
          ```
          - Remember when working with subclasses that `private`
            members are never inherited, and package members are
            only inherited if the two classes are in the same package.
- **Applying Class Access Modifiers**
  Like variables and methods, you can apply access modifiers
  to classes. As you might remember from Chapter 1, a top-
  level class is one not defined inside another class. Also
  remember that a `.java` file can have at most one `public` top-
  level class.
  - While you can only have one `public` top-level class, you can
    have as many classes (in any order) with package access as
    you want. In fact, you don’t even need to declare a `public`
    class! The following declares three classes, each with
    package access:
    ```java
    // Bear.java
    class Bird {}
    class Bear {}
    class Fish {}
    ```
    - Trying to declare a top-level class with `protected` or `private` class will 
    lead to a compiler error, though.
      ```java
      // ClownFish.java
      protected class ClownFish{} // DOES NOT COMPILE
      // BlueTang.java
      private class BlueTang {} // DOES NOT COMPILE
      ```
      - **Accessing the `this` Reference**
        What happens when a method parameter has the same
        name as an existing instance variable? 
        What do you think the following program prints?
        ```java
        public class Flamingo {
          private String color = null;
          public void setColor(String color) {
            color = color;
          }
          public static void main(String… unused) {
            var f = new Flamingo();
            f.setColor("PINK");
            System.out.print(f.color);
          }
        }
        ```
        - If you said `null`, then you’d be correct. Java uses the most
        granular scope, so when it sees `color = color`, it thinks you
        are assigning the method parameter value to itself (not the
        instance variable). The assignment completes successfully
        within the method, but the value of the instance variable
        `color` is never modified and is `null` when printed in the
        `main()` method.
          - The fix when you have a local variable with the same name
          as an instance variable is to use the `this` reference or
          keyword. The `this` reference refers to the current instance
          of the class and can be used to access any member of the
          class, including inherited members. It can be used in any
          instance method, constructor, or instance initializer block.
          It cannot be used in a `static` method or `static` initializer block.
- We apply `this` to our previous method implementation as follows:
  ```java
  public void setColor(String color) {
    // Sets the instance variable with method parameter
    this.color = color; 
  }
  ```
  The corrected code will now print `PINK` as expected. In many
  cases, the `this` reference is optional. If Java encounters a variable 
  or method it cannot find, it will check the class hierarchy to see if it
  is available.
    - Now let’s look at some examples that aren’t common but that you might see on the exam:
      ```java
      1: public class Duck {
      2:   private String color;
      3:   private int height;
      4:   private int length;
      5:
      6:   public void setData(int length, int theHeight) {
      7:     length = this.length; // Backwards -- no good!
      8:     height = theHeight; // Fine, because a different name
      9:     this.color = "white"; // Fine, but this. reference not necessary
      10: }
      11:
      12:   public static void main(String[] args) {
      13:     Duck b = new Duck();
      14:     b.setData(1,2);
      15:     System.out.print(b.length + " " + b.height + " " + b.color);
      16: } }
      ```
      - This code compiles and prints the following:
        `0 2 white`
        Line 7 is incorrect. The instance variable `length` starts out with a `0` value. That `0` is
        assigned to the method parameter `length`. The instance variable stays at `0`. Line 8 
        is more straightforward. The parameter `theHeight` and instance variable `height` have
        different names. Since there is no naming collision, this is not required. Finally, line 9 
        shows that a variable assignment is allowed to use the `this` reference even when
        there is no duplication of variable names.
        - **Calling the `super` Reference**
          In Java, a variable or method can be defined in both a
          parent class and a child class. This means the object
          instance actually holds two copies of the same variable with
          the same underlying name. When this happens, how do we
          reference the version in the parent class instead of the
          current class? Let’s take a look at this example:
            - ```java
              // Reptile.java
              1: public class Reptile {
              2:   protected int speed = 10;
              3: }
              // Crocodile.java
              1: public class Crocodile extends Reptile {
              2:   protected int speed = 20;
              3:   public int getSpeed() {
              4:     return speed;
              5:   }
              6:   public static void main(String[] data) {
              7:     var croc = new Crocodile();
              8:     System.out.println(croc.getSpeed()); // 20
              9: } }
              ```
              - One of the most important things to remember about this
              code is that an instance of `Crocodile` stores two separate
              values for `speed`: one at the `Reptile` level and one at the
              `Crocodile` level. **On line 4, Java first checks to see if there is
              a local variable or method parameter named `speed`. Since
              there is not, it then checks `this.speed`; and since it exists,
              the program prints `20`.**
- But what if we want the program to print the value in the
`Reptile` class? Within the `Crocodile` class, we can access the
parent value of `speed` instead, by using the `super` reference
or keyword. The `super` reference is similar to the `this`
reference, except that it includes only members found in
the parent class.
  - ```java
    3: public int getSpeed() {
    4:   return super.speed; //Causes the program to now print 10
    5: }
    ```
    - Let’s see if you’ve gotten the hang of `this` and `super`. What
    does the following program output?
      - ```java
        1: class Insect {
        2:   protected int numberOfLegs = 4;
        3:   String label = "buggy";
        4: }
        5:
        6: public class Beetle extends Insect {
        7:     protected int numberOfLegs = 6;
        8:     short age = 3;
        9:   public void printData() {
        10:     System.out.println(this.label);
        11:     System.out.println(super.label);
        12:     System.out.println(this.age);
        13:     System.out.println(super.age);
        14:     System.out.println(numberOfLegs);
        15:   }
        16:   public static void main(String []n) {
        17:     new Beetle().printData();
        18:   }
        19: }
        ```
        - That was a trick question—this program code would not
        compile! Let’s review each line of the `printData()` method.
        Since `label` is defined in the parent class, it is accessible via
        both `this` and `super` references. For this reason, lines 10 and
        11 compile and would both print `buggy` if the class compiled.
        On the other hand, the variable `age` is defined only in the
        current class, making it accessible via `this` but not `super`.
        For this reason, line 12 compiles (and would print `3`), but
        line 13 does not. **Remember, while `this` includes current
        and inherited members, `super` only includes inherited
        members.**
          -  What would line 14 print if line 13 was commented out? 
            Even though both `numberOfLegs` variables are accessible 
            in `Beetle`, Java checks outward, starting with the narrowest 
            scope. For this reason, the value of `numberOfLegs` in the 
            `Beetle` class is used, and `6` is printed. In this example, 
            `this.numberOfLegs` and `super.numberOfLegs` refer to different 
            variables with distinct values.
              - Since `this` includes inherited members, you often only use
              `super` when you have a naming conflict via inheritance. For
              example, you have a method or variable defined in the
              current class that matches a method or variable in a parent
              class. 