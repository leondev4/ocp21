---

markmap:
  initialExpandLevel: 1
---
# **Implementing Generic Interfaces**
- Just like a class, an interface can declare a formal type parameter.
  - ```java
    public interface Shippable<T> {
      void ship(T t);
    }
    ```
- There are three ways a class 
can implements this interface.
  - Create a generic class
    - ```java
      class ShippableAbstractCrate<U> implements Shippable<U> {
        public void ship(U t) { }
      }
      ```
      - The type parameter could have been named anything, including `T`. 
      We used `U` in the example to avoid confusion about what `T` refers to.
  - Specify the generic type in the class.
    - ```java
      class ShippableRobotCrate implements Shippable<Robot> {
        public void ship(Robot t) { }
      }
      ```
      - There is no `<>` after class name. Same as 
      implementing `Comparable` in your class.
  - Not use generics at all
    - ```
      class ShippableCrate implements Shippable {
        public void ship(Object t) { }
      }
      ```
- #### What You Can’t Do with Generic Types
  - Call a constructor: Writing `new T()`
  - Create an array of that generic type: `T[] t= new T[2];`
  - Call `instanceof`: at runtime `List<Integer>` and
  `List<String>` look the same to Java
  - Use a primitive type as a generic type parameter: `List<int>`
  - Create a `static` variable as a generic type parameter:
    - ```java 
      public class Example<T> {
          // This is not allowed:
          public static T obj;// Compile error 
      }
      ```
  - Create a `static` method and return a generic 
  type parameter or create method parameter:
    - ```
      public class Example<T> {
        // This is not allowed:
        public static T method(T obj) { }// Compile error
        //public static <T> T method(T obj){}//ok 
      }
      ```
  - Create exceptions with generic types
    - ```
      class MyExcepcion<T> extends Exception { }// compile error
      ```
  - Create annotations with generic types