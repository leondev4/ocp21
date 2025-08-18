---

markmap:
  initialExpandLevel: 1
---
# **Functional Interfaces**
- A *functional interface* is an interface that contains a single abstract 
method. It is  known as a *single abstract method (SAM)* rule.
  - The `@FunctionalInterface` annotation tells the compiler 
  that you intend for the code to be a functional interface.
  - Remember that having exactly one abstract method is
   what makes it a functional interface, not the annotation.
   - Only apply to **interfaces**
- Example
  - ```java
    @FunctionalInterface
    public interface Sprint {
      public void sprint(int speed);
    }
    ```
- There is one exception to the 
  single abstract method rule
  - If a functional interface includes an abstract method with the 
  same signature as a `public` method found in `Object`, *those 
  methods do not count toward the single abstract method test*.
    - ```java
      public String toString()
      ```
    - ```java
      public boolean equals(Object)
      ```
      - ```java
        public interface Dive{
          String toString();
          public boolean equals(Object o);
          public abstract int hashCode();
          public void dive(); //SAM method
        }
      ```
    - ```java
      public int hashCode()
      ```
  - You also cannot declare an interface method that is incompatible with 
    `Object`. For example, declaring an abstract method `int toString()` in 
    an interface would not compile since `Object`’s version of the method
    returns a `String`.
    - They are not a functional interfaces
      - ```java
        public interface Soar {
          abstract String toString(); 
        }
        ```
        - Since `toString()` is a `public` method implemented in `Object`, 
        it does not count toward the single abstract method test. 
      - ```java
        public interface Hibernate{
          String toString();
          public boolean equals(Hibernate o);
          public abstract int hashCode();
          public void rest();
        }
        ```
        - Because uses `equals(Hibernate)` instead of `equals(Object)`
- Starting with Java 8, interfaces can have concrete methods.
  - ```java
    public default
    ```
  - ```java
    public static
    ```
  - Starting with java 9
    - `private static`
    - `private`