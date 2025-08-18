---

markmap:
  initialExpandLevel: 2
---
# **Checked Exceptions**
- Must be declared or handled
- All inherit `Exception` but not `RuntimeException`. 
- Tend to be more anticipated; for example, trying to read a file that doesn’t exist.
- _Handle or declare rule_
  - All checked exceptions that could be thrown within a 
  method are either wrapped in compatible `try` and 
  `catch` blocks or declared in the method signature.
- Examples
  - **Declare:**
    ```
    void fall(int distance) throws IOException {
      if(distance > 10) {
        throw new IOException();
      }
    }
    ``` 
    - `throws` keyword simply declares that the method might throw an 
    `IOException`.  **It also might not**.

    - `throw` keyword tells Java that you want to throw an `IOException`.
  - **Handle:**
    ```js
    void fall(int distance) {
      try {
        if(distance > 10) {
            throw new IOException();
          }
        } catch (Exception e) {
          e.printStackTrace();
        }
    }
    ```
    - Since `IOException` is a subclass of `Exception`, the catch block is allowed to catch it.
# **Unchecked Exceptions**
- An _unchecked exception_ is any exception that 
does not need to be declared or handled
- Often referred to as _runtime exceptions_,
-  in Java, unchecked exceptions include any class 
that inherits `RuntimeException` or `Error`.
- It is permissible to handle or declare an unchecked
 exception, but it isn't mandatory.
- A _runtime exception_ is defined as the `RuntimeException`
 class and its subclasses
  - For example, a `NullPointerException` can be thrown in the 
  body of the following method if the `input` reference is `null`:
    ```
    void fall(String input{   
      System.out.println(input.toLowerCase());
    }
    ```
# **Error and Throwable**
- `Error` means something went so horribly wrong that 
your program should not attempt to recover from it
  - For example, the disk drive “disappeared” 
  or the program ran out of memory.
- `Throwable` is the parent class of all 
exceptions, including the `Error` class.
# **Reviewing Exception Types**
  - |Type|How to recognize|OK for program<br/> to catch?|Is program required to<br/> handle or declare?|
    |-|-|-|-|
    |Unchecked exception|Subclass of `RuntimeException`|Yes|No|
    |Checked exception|Subclass of `Exception` but not subclass of<br/>`RuntimeException`|Yes|Yes|
    |Error|Subclass of `Error`|No|No|