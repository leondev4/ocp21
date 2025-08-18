---

markmap:
  initialExpandLevel: 2
---
# **Throwing an Exception**
- The `throw` keyword tells Java that you want some 
other part of the code to deal with the exception. 
  - Remember: `throws` keyword is used only at the end of a
   method declaration to indicate what exceptions it supports.
- The first way is code that’s wrong.
  - ```js
    String[] animals = new String[0];
    System.out.println(animals[0]); //  ArrayIndexOutOfBoundsException
    ```
- The second way is for code that result in an exception 
is to explicitly  request Java to throw one.
  - ```java
    throw new Exception();
    throw new Exception("Ow! I fell.");
    throw new RuntimeException();
    throw new RuntimeException("Ow! I fell.");
    ```
- ```java
  var e = new RuntimeException(); 
  throw e;
  ```
  - The code instantiates an exception on 
  one line and then throws on the next.
- ```js
  throw RuntimeException(); // DOES NOT COMPILE
  ```
  - The exception is never instantiated with `new`
- ```js
  3: try {
  4:   throw new RuntimeException();
  5:   throw new ArrayIndexOutOfBoundsException(); // DOES NOT COMPILE
  6: } catch (Exception e) {}
  ```
  - Line 5 can never be reached during runtime.
Unreachable code error.
# **Calling Methods That Throw <br/>Checked Exceptions**
- ```
  class NoMoreCarrotsException extends Exception {}

  public class Bunny {
    public void hopAround() {
      eatCarrot(); // DOES NOT COMPILE
    }
    private void eatCarrot() throws NoMoreCarrotsException {}
  }
  ```
  - You must handle or declare. `NoMoreCarrotsException`
is checked exception
  - You might have noticed that `eatCarrot() `didn’t throw an
  exception; it just declared that it could. This is enough 
  for the compiler to require the caller to handle or declare 
  the exception.
  - Declaring an unused exception isn’t considered unreachable
    code. It gives the method the option to change the
    implementation to throw that exception in the future.
- The above code compiles if either 
of the two options is used
  - ```js
    public void hopAround() throws NoMoreCarrotsException{
      eatCarrot(); // It compiles
    }
    ```
  - ```js
    public void hopAround() {
      try {
        eatCarrot();
      } catch (NoMoreCarrotsException e) {
        System.out.print("sad rabbit");
      }
    }
    ```
- Java knows that `eatCarrot()` can’t throw a checked
 exception, ­which means there’s no way for the 
 catch block in `bad()` to be reached.
  - ```
    public class Bunny {
      public void bad() {
        try {
          eatCarrot(); //It Must throws checked exception NoMoreCarrosException
        } catch (NoMoreCarrotsException e) { // DOES NOT COMPILE
          System.out.print("sad rabbit");  //unreachable code
        }
      }
      private void eatCarrot() {}
    }
    ```
- When you see a checked exception declared inside a `catch` block on the exam,
make sure the code in the associated `try` block is capable of throwing the 
exception or a subclass of the exception. If not, the code is unreachable and 
does not compile.