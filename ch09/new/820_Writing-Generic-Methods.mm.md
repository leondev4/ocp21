---

markmap:
  initialExpandLevel: 1
---
# **Writing Generic Methods**
- You've seen formal type parameters declared on the `class` or `interface` level. 
It is also possible to declare them on the method level. This is _often useful for 
`static` methods_ since they aren’t part of an instance that can declare the type. 
However, it is also allowed on non-`static` methods.
- ```java
  public class Handler {
    public static <T> void prepare(T t) {
      System.out.println("Preparing " + t);
    }
    public static <T> Box<T> ship(T t) {
      System.out.println("Shipping " + t);
      return new Box<T>();
    }
  }
  ```
  - Before the return type, we declare formal parameter type of `<T>`
  - In the `ship()` method, we show how you can use the generic 
  parameter in the return type, `Box<T>`, for the method.
  - `T` is a different type in both methods
- Unless a method is obtaining the generic formal type
  parameter from the class/interface, it is specified
  immediately before the return type of the method.
  - ```java
    public class More {
      public static <T> void sink(T t) { }
      public static <T> T identity(T t) { return t; }
      public static T noGood(T t) { return t; } // DOES NOT COMPILE
      //public static <T> T noGood(T t) { return t; }
    }
    ```
- Optional Syntax for Invoking a Generic Method
  - ```
    Box.<String>ship("package");
    Box.<String[]>ship(args);
    ```
  - When you have a method that declare a 
  generic parameter type, it is independent 
  of the class generics.
    ```java
    1: public class TrickyCrate<T> {
    2:   public <T> T tricky(T t) {
    3:     return t;
    4:    }
    5: }
    ```
    - ```java
      10: public static String crateName() {
      11:   TrickyCrate<Robot> crate = new TrickyCrate<>();
      12:   return crate.tricky("bot");
      13: }
      ```
    - On line 1, `T` is `Robot` because that is what gets referenced 
    when constructing a `TrickyCrate`. On line 2, `T` is `String` because 
    that is what is passed to the method.
- ### Creating a Generic Record
  - Generics can also be used with records. This record takes a single 
  generic type parameter:
    ```java
    public record CrateRecord<T>(T contents) {
      @Override
      public T contents() {
        if (contents == null)
          throw new IllegalStateException("missing contents");
        return contents;
      }
    }
    ```
    - This works the same way as classes. You can create a record of the robot!
      ```java
      Robot robot = new Robot();
      CrateRecord<Robot> record = new CrateRecord<>(robot);
      var record2 = new CrateRecord<Robot>(robot);
      ```