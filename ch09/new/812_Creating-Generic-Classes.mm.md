---

markmap:
  initialExpandLevel: 1
---
# **Creating Generic Classes**
- The syntax for introducing a generic is to declare 
  a _formal type parameter_ in angle brackets (`<>`).
  - ```java
    public class Box<T>{
      private T contents;
      public T lookInBox() {
        return contents;
      }
      public void packBox(T contents) {
        this.contents = contents;
      }
    }
    ```
    - The generic type **`T`** is available anywhere within the `Box` class.
    When you instantiate the class, you tell the compiler what **`T`** 
    should be for that particular instance.
    - To use, type:
      ```java
      Box<String> crateForString = new Box<>();
      boxForString.packBox("hello");
      String recover = boxForString.lookInBox();
      ```
      - The `Box` class works with any type of class.
      - `Box` not needing to know about the objects that go into it, those objects 
      don’t need to know about `Box`. We aren’t requiring the objects to 
      implement an interface named `Crateable` or the like. A class can be put 
      in the `Box` without any changes at all.
- Generic classes aren’t limited to having a single type parameter.
  - ```java
    public class SizeLimitedCrate<T, U> {
      private T contents;
      private U sizeLimit;
      public SizeLimitedCrate(T contents,U sizeLimit) {
        this.contents = contents;
        this.sizeLimit = sizeLimit;
      }}
    ```
- Type erasure:
  - After the code compiles, your 
  generics are just `Object` types
    - The compiler adds the relevant casts for your 
    code to work with this type of erased class.
      - ```java
        public class Crate {
          private Object contents;
          public Object lookInCrate() {
            return contents;
          }
          public void packCrate(Object contents) {
            this.contents = contents;
          }
        }
        ```
        -  For example, you type the following:
            ```java
            Robot r = crate.lookInCrate();
            ```
            The compiler turns it into the following:
            ```java
            Robot r = (Robot) crate.lookInCrate();
            ```
- Naming Conventions for Generics
  - `E` for an element
  - `K` for a map key
  - `V` for a map value
  - `N` for a number
  - `T` for a generic data type
  - `S`, `U`, `V`, and so forth for multiple generic types