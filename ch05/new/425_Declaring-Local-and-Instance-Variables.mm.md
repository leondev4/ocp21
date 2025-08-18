---

markmap:
  initialExpandLevel: 1
---
# **Declaring Local and Instance Variables**
- Local variables are those defined within a method or block,
while instance variables are those that are defined as a
member of a class. Let’s take a look at an example:
  - ```java
    public class Lion {
      int hunger = 4; // Instance variable
      public int feedZooAnimals() {
        int snack = 10; // Local variable
        if(snack> 4) {
          long dinnerTime = snack++;// Local variable
          hunger--;
        }
        return snack;
      }
    }
    ```
    - In the `Lion` class, `snack` and `dinnertime` are local variables
  accessible only within their respective code blocks, while
  `hunger` is an instance variable and created in every object 
  of the `Lion` class.
        - The object or value returned by a method may be available
        outside the method, but the primitive variable `snack` is gone. 
        All local variables are destroyed after the block is executed, 
        but the objects they point to may still be accessible.
- **Local Variable Modifiers**
There’s only one modifier that can be applied to a local
variable: `final`. In this code sample, trying to change the
value or object these variables reference results in a
compiler error:
    - ```java
      public void zooAnimalCheckup(boolean isWeekend) {
        final int rest;
        if(isWeekend) rest = 5; else rest = 20;
        System.out.print(rest);
        final var giraffe = new Animal();
        final int[] friends = new int[5];
        rest = 10;              // DOES NOT COMPILE
        giraffe = new Animal(); // DOES NOT COMPILE
        friends = null;         // DOES NOT COMPILE
      }
      ```
      - As shown with the `rest` variable, we don’t need to assign a
      value when a `final` variable is declared. **The rule is only
      that it must be assigned a value before it can be used**. 
      We can even use `var` and `final` together.
        - Contrast this with the following example:
          ```java
          public void zooAnimalCheckup(boolean isWeekend) {
            final int rest;
            if(isWeekend) rest = 5;
            System.out.print(rest); // DOES NOT COMPILE
          }
          ```
          - The `rest` variable might not have been assigned a value,
          such as if `isWeekend` is `false`. Since the **compiler does not
          allow the use of local variables that may not have been
          assigned a value**, the code does not compile.
            - Does using the `final` modifier mean we can’t modify the
            data? Nope. **The `final` attribute refers only to the variable
            reference; the contents can be freely modified** (assuming
            the object isn’t immutable).
              ```java
              public void zooAnimalCheckup() {
                final int rest = 5;
                final Animal giraffe = new Animal();
                final int[] friends = new int[rest];

                giraffe.setName("George");
                friends[2] = 2;
              }
              ```
              - The `rest` variable is a primitive, so it’s just a value that can’t
              be modified. On the other hand, the contents of the `giraffe`
              and `friends` variables can be freely modified, provided the
              variables aren’t reassigned.
- **Effectively Final Variables**
  An _effectively final_ local variable is one that is not modified
  after it is assigned. This means that the value of a variable
  doesn’t change after it is set, regardless of whether it is
  explicitly marked as `final`.
    - Given this definition, which of the following variables 
    are effectively final?
      ```java
      11: public String zooFriends() {
      12:   String name = "Harry the Hippo";
      13:   var size = 10;
      14:   boolean wet;
      15:   if(size> 100) size++;
      16:   name.substring(0);
      17:   wet = true;
      18:   return name;
      19: }
      ```
      - Remember, a quick test of effectively final is to just add `final` 
      to the variable declaration and see if it still compiles. In this
        example, `name` and `wet` are effectively final and can be
      updated with the `final` modifier, but not `size`. The `name` 
      variable is assigned a value on line 12 and not reassigned. 
      Line 16 creates a value that is never used. Remember that
        strings are immutable. The `size` variable is not effectively 
        final because it could be incremented on line 15. The `wet` 
        variable is assigned  a value only once and not modified 
        afterward.
- **Instance Variable Modifiers**
Like methods, instance variables can have different access
levels, such as `private`, package, `protected`, and `public`.
Remember, package access is indicated by the lack of any
modifiers. Instance variables can also use optional specifiers,
described in **Table 5.3**.
  - **TABLE 5.3** Optional specifiers for instance variables
    |Modifier|Description|
    |-|-|
    |`final`|Specifies that the instance variable must be initialized <br/>with each instance of the class exactly once|
    |`volatile`|Instructs the JVM that the value in this variable may be<br/> modified by other threads|
    |`transient`|Used to indicate that an instance variable should not<br/>be serialized with the class|
    - We only discuss `final` in this chapter! If an instance
    variable is marked `final`, then it must be assigned
     a value when it is declared or when the object is
    instantiated. Like a local `final` variable, it cannot be
    assigned a value more than once, though. 
      -  The following `PolarBear` class demonstrates
       these properties:
          ```java
          public class PolarBear {
            final int age = 10;
            final int fishEaten;
            final String name;

            { fishEaten = 10; }

            public PolarBear() {
              name = "Robert";
            }
          }
          ```
          - The `age` variable is given a value when it is **declared**, while
          the `fishEaten` variable is assigned a value in an **instance
          initializer**. The `name` variable is given a value in the **no-
          argument constructor**. Failing to initialize an instance
          variable (or assigning a value more than once) will lead to a
          compiler error. We talk about `final` variable initialization in
          more detail in the next chapter.
            - **NOTE:**
             The compiler does not apply a default value to `final`
          variables, though. A `final` instance or `final static`
          variable must receive a value when it is declared or as
          part of initialization.
- **Creating Methods with Varargs**
There are a number of important rules for creating a
method with a varargs parameter.
  - **Rules for Creating a Method with a Varargs Parameter**
  1.- A method can have at most one varargs parameter.
  2.- If a method contains a varargs parameter, it must be  
  &nbsp;  &nbsp;  &nbsp;  the last parameter in the list.
    - Given these rules, can you identify why each of these does or doesn’t 
    compile? 
      ```java
      public class VisitAttractions {
        void walk1(int… steps) {} // rule 1
        void walk2(int start, int… steps) {} // rule 2
        void walk3(int… steps, int start) {} // DOES NOT COMPILE
        void walk4(int… start, int… steps) {} // DOES NOT COMPILE
      }
      ```
      - The `walk1()` method is a valid declaration with one varargs
    parameter. The `walk2()` method is a valid declaration with
    one `int` parameter and one varargs parameter. The `walk3()`
    and `walk4()` methods do not compile because they have a
    varargs parameter in a position that is not the last one.
        - **Calling Methods with Varargs**
        When calling a method with a varargs parameter, you have
        a choice. **You can pass in an array, or you can list the
        elements of the array** and let Java create it for you. Given
        our previous `walk1()` method, which takes a varargs
        parameter, we can call it one of two ways:
          ```java
          // Pass an array
          int[] data = new int[] {1, 2, 3};
          walk1(data);

          // Pass a list of values
          walk1(1,2,3);
          ```
          - Regardless of which one you use to call the method, the
          method will receive an array containing the elements. We
          can reinforce this with the following example:
            ```java
            public void walk1(int… steps) {
              // It shows steps is of type int[]
              int[] step2 = steps;
                System.out.print(step2.length);
            }
            ```
            You can even omit the varargs values in the method call,
            and **Java will create an array of length zero for you**.
            `walk1();`
              - **Accessing Elements of a Vararg**
                Accessing a varargs parameter is just like accessing an
                array. It uses array indexing. Here’s an example:
                ```java
                16: public static void run(int… steps) {
                17:   System.out.print(steps[1]);
                18: }
                19: public static void main(String[] args) {
                20:   run(11, 77); // 77
                21: }
                ```
                Line 20 calls a varargs method with two parameters. When
                the method is called, it sees an array of size `2`. Since
                indexes are zero-based, `77` is printed.
- **Using Varargs with Other Method Parameters**
 Can you figure out why each method call outputs what it does?
  ```java
  1:  public class DogWalker {
  2:  public static void walkDog(int start, int… steps) {
  3:    System.out.println(steps.length);
  4:  }
  5:  public static void main(String[] args) {
    6:  walkDog(1); // 0
    7:  walkDog(1, 2); // 1
    8:  walkDog(1, 2, 3); // 2
    9:  walkDog(1, new int[] {4, 5}); // 2
  10: } }
  ```
  - Line 6 passes `1` as `start` but nothing else. This means Java
  creates an array of length `0` for `steps`. Line 7 passes `1` as
  `start` and one more value. Java converts this one value to an
  array of length `1`. Line 8 passes `1` as start and two more
  values. Java converts these two values to an array of length `2`. 
  Line 9 passes `1` as `start` and an array of length `2` directly as 
  `steps`.
    - You’ve seen that Java will create an empty array if no
      parameters are passed for a vararg. However, it is still
      possible to pass `null` explicitly. The following snippet 
      does compile:
      ```java
      walkDog(1,null);
      // Triggers NullPointerException in walkDog()
      ```
      Since `null` isn’t an `int`, Java treats it as an array reference
      that happens to be `null`. It just passes on the `null` array
      object to `walkDog()`. Then the `walkDog()` method throws an
      exception because it tries to determine the length of `null`.