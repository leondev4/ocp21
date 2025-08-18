---

markmap:
  initialExpandLevel: 1
---

# **Designing Methods**
- You can write a basic method to take a nap, as shown in 
[Figure 5.1](https://1drv.ms/i/c/c83cfca51d5c2032/EY3Jjb0QoLRDrYC3MosRq1kBFQlBx7wGVJVNlGbHk9zLRg?e=fEiL6Q).
  - This is called a _method declaration_, which specifies all the
  information needed to call the method. Two of the parts
  the method **name** and **parameter list** are called the
  **method signature**. The method signature provides
  instructions for how callers can reference this method. The
  method signature only includes name and parameter list.
  Access modifiers, control where the method can be
  referenced.
    - **TABLE 5.1** Parts of a method declaration in **Figure 5.1**
      |Element|Value in `nap()`<br/>example|Required?|
      |-|-|-|
      |Access modifier|`public`| No|
      |Optional specifier|`final`|No|
      |Return type|`void`|Yes|
      |Method name|`nap`|Yes|
      |Parameter list|`(int minutes)`|Yes, but can be empty parentheses|
      |Method signature|`nap(int minutes)`|Yes|
      |Exception list| `throws InterruptedException`|No|
      |Method body |`{ // take a nap`<br/>`}`|Yes, except for `abstract` methods|
        - To call this method, just use the method signature and
provide an `int` value in parentheses:
        `
        nap(10);
        `
- **Access Modifiers**
An access modifier determines what classes a method can
be accessed from. Think of it like a security guard.
 Java offers four choices of access:
  - **private** The `private` modifier means the method can
  be called only from within the same class.
  **Package Access** With package access, the method
  can be called only from a class in the same package.
  You only omit the access modifier. Package access is
  sometimes referred to as package-private or default
  access.
  **protected** The `protected` modifier means the method
  can be called only from a class in the same package or
  a subclass.
  **public** The `public` modifier means the method can be
  called from anywhere.
    -  Make sure you understand why each of these is a valid or 
    invalid method declaration. Pay attention to the access
    modifiers as you figure out what is wrong with the ones
    that don’t compile when inserted into a class:
        ```java
        public class ParkTrip {
          public void skip1() {}
          default void skip2() {} // DOES NOT COMPILE
          void public skip3() {} // DOES NOT COMPILE
          void skip4() {}
        }
        ```
        - The `skip1()` method is a valid declaration with `public` access.
        The `skip4()` method is a valid declaration with package access. 
        The `skip2()` method doesn’t compile because `default` is not a 
        valid access modifier. There is a `default` keyword, which is used 
        in `switch` statements and interfaces, but `default` is never used 
        as an access modifier. The `skip3()` method doesn’t compile 
        because the access modifier is specified after the return type.
- **Optional Specifiers**
There are a number of optional specifiers for methods,
shown in **Table 5.2**. Unlike with access modifiers, you can
have multiple specifiers in the same method (although not
all combinations are legal). When this happens, you can
specify them in any order. And since these specifiers are
optional, you are allowed to not have any of them at all.
  - **TABLE 5.2** Optional specifiers for methods
    |Modifier|Description|
    |-|-|
    |`static`|Indicates the method is a member of the shared class object|
    |`abstract`|Used in an abstract class or interface when the method body is <br/>excluded|
    |`final`|Specifies that the method may not be overridden in a subclass|
    |`default`|Used in an interface to provide a default implementation of a<br/>method for classes that implement the interface|
    |`synchronized`|Used with multithreaded code|
    |`native`|Used when interacting with code written in another language, such<br/>as C++|
    |`strictfp`|Used for making floating-point calculations portable|
    - As you can see in **Table 5.2**, the first is convered here, but
     the next four of the method modifiers are covered in later
      chapters, and the last two aren’t even in scope for the exam.
      Remember, access modifiers and optional specifiers can
      be listed in any order, but once the return type is specified,
      the rest of the parts of the method are written in a specific
      order: name, parameter list, exception list, body.
      Some specifiers are not compatible with one another. For 
      example, you can’t declare a method (or class) both `final` 
      and `abstract`.
        - Again, just focus on syntax for now. Do you see why these
compile or don’t compile?
          ```java
          public class Exercise {
            public void bike1() {}
            public final void bike2() {}
            public static final void bike3() {}
            public final static void bike4() {}
            public modifier void bike5() {} // DOES NOT COMPILE
            public void final bike6() {} // DOES NOT COMPILE
            final public void bike7() {}
          }
          ```
          - The `bike1()` method is a valid declaration with no optional
          specifier. The `bike2()` method is a valid declaration, with 
          `final` as the optional specifier. The `bike3()` and `bike4()` 
          methods are valid declarations with both `final` and `static` 
          as optional specifiers. The order of these two keywords
           doesn’t matter. The `bike5()` method doesn’t compile 
           because `modifier` is not a valid optional specifier. The 
           `bike6()` method doesn’t compile because the optional 
           specifier is after the return type.
           The `bike7()` method does compile. Java allows the optional
            specifiers to appear before the access modifier. 
- **Return Type**
The next item in a method declaration is the return type. It
must appear after any access modifiers or optional
specifiers and before the method name. The return type
might be an actual Java type such as `String` or `int`. If there is
no return type, the `void` ("without contents") keyword is used.
  - When checking return types, you also have to look inside
the method body. Methods with a return type other than
`void` are required to have a `return` statement inside the
method body. This `return` statement must include the
primitive or object to be returned. Methods that have a
return type of `void` are permitted to have a `return` statement 
(it ends early) with no value returned or omit the `return`
 statement entirely.
    - Example:
      ```java
      public void swim(int distance) {
        if(distance <= 0) {
          // Exit early, nothing to do!
          return;
        }
        print("Fish is swimming "+distance+"meters");
      }
      ``` 
      - Ready for some examples? Can you explain why these
      methods compile or don’t?
        ```java
        public class Hike {
          public void hike1() {}
          public void hike2() { return; }
          public String hike3() { return ""; }
          public String hike4() {} // DOES NOT COMPILE
          public hike5() {} // DOES NOT COMPILE
          public String int hike6() { } // DOES NOT COMPILE
          String hike7(int a) { // DOES NOT COMPILE
            if (1 < 2) return "orange";
          }
        }
        ```
        - Since the return type of the `hike1()` method is `void`, the
        `return` statement is optional. The `hike2()` method shows the
        optional `return` statement that correctly doesn’t return
        anything. The `hike3()` method is a valid declaration with a
        `String` return type and a return statement that returns a
        `String`. The `hike4()` method doesn’t compile because the
        `return` statement is missing. The `hike5()` method doesn’t
        compile because the return type is missing. The `hike6()`
        method doesn’t compile because it attempts to use two
        return types. 
          - The `hike7()` method is a little tricky. There is a `return`
          statement, but it doesn’t always get run. Even though `1` is
          always less than `2`, the compiler won’t fully evaluate the if
          statement and requires a `return` statement if this condition
          is `false`. What about this modified version?
            ```java
            String hike8(int a) {
              if (1 < 2) return "orange";
              return "apple"; // COMPILER WARNING
            }
            ```
            The code compiles, although the compiler will produce a
            warning about unreachable code (or dead code). This
            means the compiler was smart enough to realize you wrote
            code that cannot possibly be reached
              - When returning a value, it needs to be assignable to the
              return type. Can you spot what’s wrong with two of these
              examples?
                ```java
                public class Measurement {
                  int getHeight1() {
                    int temp = 9;
                    return temp;
                  }
                  int getHeight2() {
                    int temp = 9L; // DOES NOT COMPILE
                    return temp;
                  }
                  int getHeight3() {
                    long temp = 9L;
                    return temp; // DOES NOT COMPILE
                  }
                }
                ```
                - The `getHeight2()` method doesn’t compile because you can’t
                assign a `long` to an `int`. The `getHeight3()` method doesn’t
                compile because you can’t return a `long` value as an `int`.
- **Method Name**
An identifier may only contain letters, numbers, currency
symbols, or `_`. Also, the first character is not allowed to be a
number, and reserved words are not allowed. Finally, the
single underscore character is not allowed.

  - By convention, methods begin with a lowercase letter, but
  they are not required to. We can jump right into practicing 
  with some examples:
    ```java
    public class BeachTrip {
      public void jog1() {}
      public void 2jog() {} // DOES NOT COMPILE
      public jog3 void() {} // DOES NOT COMPILE
      public void Jog_$() {}
      public _() {} // DOES NOT COMPILE
      public void() {} // DOES NOT COMPILE
    }
    ```
    - The `jog1()` method is a valid declaration with a traditional
    name. The `2jog()` method doesn’t compile because
    identifiers are not allowed to begin with numbers. The
    `jog3()` method doesn’t compile because the method name is
    before the return type. The `Jog_$()` method is a valid
    declaration, it is legal. The `_` method is not allowed since it
    consists of a single underscore. The final line of code doesn’t
    compile because the method name is missing.
      - **Parameter List**
      Although the parameter list is required, it doesn’t have to
      contain any parameters. This means you can just have an
      empty pair of parentheses after the method name, as
      follows:
        ```java
        public class Sleep {
          void nap() {}
        }
        ```
        - If you do have multiple parameters, you separate them with a comma. 
        For now, let’s practice looking at method declarations with “regular”
        parameters:
          ```java
          public class PhysicalEducation {
            public void run1() {}
            public void run2 {} // DOES NOT COMPILE
            public void run3(int a) {}
            public void run4(int a; int b) {} // DOES NOT COMPILE
            public void run5(int a, int b) {}
          }
          ```
          - The `run1()` method is a valid declaration without any
          parameters. The `run2()` method doesn’t compile because it
          is missing the parentheses around the parameter list. The
          `run3()` method is a valid declaration with one parameter.
          The `run4()` method doesn’t compile because the parameters
          are separated by a semicolon rather than a comma.
          Semicolons are for separating statements, not for
          parameter lists. The `run5()` method is a valid declaration
          with two parameters.
- **Method Signature**
A method signature, composed of the method name and
parameter list, is what Java uses to uniquely determine
exactly which method you are attempting to call. Once it
determines which method you are trying to call, it then
determines if the call is allowed. For example, attempting
to access a `private` method outside the class or assigning
the return value of a `void` method to an `int` variable results
in compiler errors. Neither of these compiler errors is
related to the method signature, though.
  - It’s important to note that the **names of the parameters in the method
  signature are not used as part of a method signature**. The parameter 
  list is about the types of parameters and their order. For example, the 
  following two methods have the exact same signature:
    ```java
    // DOES NOT COMPILE
    public class Trip {
      public void visitZoo(String name, int waitTime) {}
      public void visitZoo(String attraction, int rainFall) {}
    }
    ```
    The method signature is:
    ```java
    visitZoo(String,int)
    ```
    - Despite having different parameter names, these two methods have 
    the same signature and cannot be declared within the same class. 
    Changing the order of parameter types does allow the method to
    compile, though:
        ```java
        public class Trip {
          public void visitZoo(String name, int waitTime) {}
          public void visitZoo(int rainFall, String attraction) {}
        }
        ```
      We cover these rules in more detail when we get to method overloading 
      later in this chapter.
      - **Exception List**
        In Java, code can indicate that something went wrong by
        throwing an exception. For now, you just need to know
        that it is optional and where in the method declaration it
        goes if present. For example, `InterruptedException` is a
        type of `Exception`. You can list as many types of exceptions
        as you want in this clause, separated by commas.
          - Here’s an example:
            ```java
            public class ZooMonorail {
              public void zeroExceptions() {}
              public void oneException() throws IllegalArgumentException
              {}
              public void twoExceptions() throws
              IllegalArgumentException, InterruptedException {}
            }
            ```
            While the list of exceptions is optional, it may be required by the compiler,
            depending on what appears inside the method body. 
              - **Method Body**
              The final part of a method declaration is the method body.
              A method body is simply a code block. It has braces that
              contain zero or more Java statements.  
                ```java
                public class Bird {
                  public void fly1() {}
                  public void fly2() // DOES NOT COMPILE
                  public void fly3(int a) { int name = 5; }
                }
                ```
                  - The `fly1()` method is a valid declaration with an empty
                  method body. The `fly2()` method doesn’t compile because it
                  is missing the braces around the empty method body.
                  Methods are required to have a body unless they are
                  declared `abstract`. The `fly3()` method is a valid declaration
                  with one statement in the method body.