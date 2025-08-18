---

markmap:
  initialExpandLevel: 1
---
# **Working with Enums**
- An enumeration, or enum for short, is like a fixed set of constants.
Using an enum is much better than using a bunch of constants because it provides 
type-safe checking. With numeric or `String` constants, you can pass an invalid value
and not find out until runtime. With enums, it is impossible to create an invalid enum
value without introducing a compiler error.
  - **Creating Simple Enums**
To create an enum, declare a type with the `enum` keyword, a name, and a list of 
values, as shown in [Figure 7.4](https://1drv.ms/i/c/c83cfca51d5c2032/EVVmO8a1KYtKrTXzyyudxnUBd7R56xWKV8vkf4My2z5asQ?e=hRBfm4).
We refer to an enum that only contains a list of values as a *simple enum*.<strong style="color:green"> When 
working with simple enums, the semicolon at the end of the list is optional</strong>.
    - Using an enum is easy.
      ```java
      var s = Season.SUMMER;
      System.out.println(Season.SUMMER);      //SUMMER
      System.out.println(s == Season.SUMMER); //true
      ```
      As you can see, enums print the name of the enum when `toString()` is called. They can 
      be compared using `==` because they are like `static final` constants. You can use 
      `equals()` or `==` to compare enums, since <strong>each enum value is initialized only once 
      in the Java Virtual Machine (JVM).</strong>
        - One thing that you can’t do is extend an enum.
          ```java
          public enum ExtendedSeason extends Season {} // DOES NOT COMPILE
          ```
          An `enum` is implicitly `final` and its constructor is implicitly `private`. You cannot 
          mark an enum `final`. On the other hand, an enum can implement an interface,
          An `enum` implicitly `extends java.lang.Enum` class.
            - **Calling Common Enum Methods**
              An enum provides a `values()` method to get an array of all of the values. You can use this like any 
              normal array, including in a for-each loop. In addition, each enum value includes two methods,
              `name()` and `ordinal()`. The following shows all three methods:
              ```java
              for(var season: Season.values()) {
                System.out.println(season.name() + " " + season.ordinal());
              }
              ```
              - The `ordinal()` method returns an `int` value, which denotes
              the order in which the value is declared in the enum:
                ```java
                WINTER 0
                SPRING 1
                SUMMER 2
                FALL 3
                ```
                -  You also can’t compare an `int` and an enum value directly anyway since an enum value is an object.
                    ```java
                    if (Season.SUMMER == 2) {} // DOES NOT COMPILE
                    ```
                    An enum provides a useful `valueOf()` method for converting from a `String` to an enum value. This 
                    is helpful when working with older code or parsing user input.<strong style="color:red"> The `String` passed in must match 
                    the  enum value exactly</strong>, though.
                    ```java
                    Season s = Season.valueOf("SUMMER"); // SUMMER
                    Season t = Season.valueOf("summer"); // IllegalArgumentException
                    ```
                    - The first statement works and assigns the proper enum value to `s`. Note that this line is not creating 
                    an enum value, at least not directly. Each enum value is created once when the enum is first loaded. 
                    Once the enum has been loaded, it retrieves the single enum value with the matching name.
                    The second statement encounters a problem. There is no enum value with the lowercase name 
                    `summer`. Java  throws an `IllegalArgumentException`.
- **Using Enums in switch Statements**
Enums can be used in `switch` statements and expressions. Enums have the unique 
property that they do not require a default branch for an exhaustive `switch` if all enum 
values are handled.
  ```java
  public String getWeather(Season value) {
    return switch (value) {
      case SUMMER -> "Too hot";
      case Season.WINTER -> "Too cold";
      case SPRING, FALL -> "Just right";
    };
  }
  ```
  - A `default` branch can also be added but is not required, so long as all values are handled. 
    Also notice that within each case clause, the name of the enum, `Season`, is now optional.
    In previous versions of Java, the name of the enum was disallowed.
      - While each enum value has an accompanying ordinal value, it cannot be used 
      directly  within a `case` clause. For example, this does not compile:
        ```java
        public String getWeather(Season value) {
          return switch (value) {
            case SUMMER -> "Too hot";
            case 0 -> "Too cold"; // DOES NOT COMPILE
            default -> "Just right";
          };
        }
- **Working with Complex Enums**
While a simple enum is composed of just a list of values, we can define a complex 
enum with additional elements. Let’s say our zoo wants to keep track of traffic 
patterns to determine which seasons get the most visitors.
  - ```java
    21: interface Visitors { void printVisitors(); }
    22: enum SeasonWithVisitors implements Visitors {
    23:   WINTER("Low"), SPRING("Medium"), SUMMER("High"), FALL("Medium");
    24:
    25:   private final String visitors;
    26:   public static final String DESCRIPTION = "Weather enum";
    27:   private SeasonWithVisitors(String visitors) { //implicit private
    28:     System.out.print("constructing,");
    29:     this.visitors = visitors;
    30:   }
    31: @Override public void printVisitors() {
    32:     System.out.println(visitors);
    33: } }
    ```
      - `WINTER`, `SPRING`, `SUMMER` and `FALL` are implicitly `public`, `static`, and `final` of type `Vistors`
        If you don't redefine the method `toString()` in a enum, the default implementation is:
        ```java
        public String toString(){  return name(); }
        ```
      - There are a few things to notice here. On line 23, the list of enum values ends with a semicolon (`;`).
      While this is optional for a simple enum, it is required if there is anything in the enum besides the
      values. Lines 25–33 are regular Java code. We have instance and `static` variables (lines 25–26), 
      a constructor (lines 27–30, note the modifier `private`), and a method (lines 31–33).
      For complex enums, the list of values always comes first.
        - **Creating Enum Variables**
          An enum declaration can include both `static` and instance variables. In our `SeasonWithVisitors` 
          implementation (lines 25–26), we mark the variables `final`, so that our enum properties cannot be 
          modified. Although it is possible to create an enum with instance variables that can be modified, 
          it is a very poor practice to do.
            - **Declaring Enum Constructors**
              <strong style="color:green">All enum constructors are implicitly `private`, with the modifier being optional.</strong> This is
              reasonable since you can’t extend an enum and the constructors can be called only within
              the enum itself. In fact, an enum constructor will not compile if it contains a `protected` or 
              `public` modifier.
              ```java
              27: public SeasonWithVisitors(String visitors) { // DOES NOT COMPILE
              ```
              - What about all of the parentheses on line 23 of our `SeasonWithVisitors` enum? Those are 
              constructor  calls, but without the `new` keyword normally used for objects. The first time we ask 
              for any of the enum  values, Java constructs all of the enum values. After that, Java just  returns 
              the already constructed  enum values.
                - Given this explanation, you can see why this code snippet calls each constructor only once:
                  ```java
                  System.out.print("begin,");
                  var firstCall = SeasonWithVisitors.SUMMER; // Prints "constructing," 4 times
                  System.out.print("middle,");
                  var secondCall = SeasonWithVisitors.SUMMER; // Doesn't print anything
                  System.out.print("end");
                  ```
                  This program prints the following:
                  ```java
                  begin,constructing,constructing,constructing,constructing,middle,end
                  ```
                  If the `SeasonWithVisitors` enum was used earlier in the program (and therefore initialized 
                  sooner), then the line that declares the `firstCall` variable would not print anything
- **Writing Enum Methods**
Like a class, an enum can contain `static` and instance methods. An enum can even 
implement an interface as we saw on lines 21–22 of our `SeasonWithVisitors` enum. 
We include the `@Override` annotation on line 31 to make it clear it is an inherited 
method. How do we call an enum instance method? That’s easy, too: we just use 
the enum value followed by the method call.
  ```java
    SeasonWithVisitors.SUMMER.printVisitors();
  ```
  - Sometimes you want to define different methods for each enum. For example, our zoo has 
  different seasonal hours. It is cold and gets dark early in the winter. We can keep track of 
  the hours through instance variables, or we can let each enum value manage hours itself.
  The syntax is similar to compact constructors in records.
    - ```java
      public enum SeasonWithTimes {
        WINTER {
          public String getHours() { return "10am-3pm"; }
        },
        SPRING {
          public String getHours() { return "9am-5pm"; }
        },
        SUMMER {
          public String getHours() { return "9am-7pm"; }
        },
        FALL {
          public String getHours() { return "9am-5pm"; }
        };
        public abstract String getHours();
      }
      ```
      - What’s going on here? It looks like we created an `abstract` class and a bunch of tiny subclasses. 
      In a way, we are. The enum itself has an `abstract` method. This means that each and every enum 
      value is required to implement this method. If we forget to implement the method for one of the 
      values, we get a compiler error:
        ```java
        The enum constant WINTER must implement the abstract method getHours()
        ```
        - But what if we don’t want each and every enum value to have a method? No problem. We can create an
          implementation for all values and override it only for the special cases.
          ```java
          public enum SeasonWithTimes {
            WINTER {
              public String getHours() { return "10am-3pm"; }
            },
            SUMMER {
              public String getHours() { return "9am-7pm"; }
            },
            SPRING, FALL;
              public String getHours() { return "9am-5pm"; }
          }
          ```
          This looks better. We only code the special cases and let the others use the enum-provided implementation.
            - The following are a few more important facts about `java.lang.Enum` which you should know:
            \* All enums implement `java.lang.Comparable` (you can add it in `SortedSet`, `TreeSet` and `TreeMap`).
            The natural order is their ordinal value.
            \* You can not clone it.
            \* Since `java.lang.Enum` class implements `java.io.Serializable`, so you get efficiente serialization 
            automatically with enumuns.
            