---

markmap:
  initialExpandLevel: 2
---

# **RuntimeException Classes**
- **`RuntimeException`** and its subclasses are unchecked 
exceptions that don’t have to be handled or declared. 
- **`ArithmeticException`**
Thrown when code attempts to divide `int` or `Integer` by zero.
  - ```java
    Integer b = 1;
    System.out.println(b/0);
    ```
    - ```
      Exception in thread "main" java.lang.ArithmeticException: / by zero
      ```
- **`ArrayIndexOutOfBoundsException`**
Thrown when code uses illegal index to access array.
  - ```java
    int []arr = {};
    System.out.println(arr[1]);
    ```
    - ```
      Exception in thread "main" 
        java.lang.ArrayIndexOutOfBoundsException: 
          Index 1 out of bounds for length 0
      ```
- **`ClassCastException`**
Thrown when attempt is made to cast object to 
class of which it is not an instance.
  - This code doesn’t compile because `Integer` is not a subclass 
  of `String`:
    ```
    String type = "moose";
    Integer number = (Integer)type; // DOES NOT COMPILE
    ```
  - When the cast fails at runtime, Java will throw a `ClassCastException`:
    ```js
    String type = "moose";
    Object obj = type;
    Integer number = (Integer)obj; // ClassCastException
    ```
    - ```
      Exception in thread "main" java.lang.ClassCastException:
        java.base/java.lang.String
          cannot be cast to java.lang.base/java.lang.Integer
      ```

      Java tells you both types that were involved in the problem
- **`NullPointerException`**
Thrown when there is a `null` reference where 
an object is required.
  - ```js
    public class Frog {
      static String name;
      public void hop() {
        System.out.print(name.toLowerCase());
      }
      public static void main(String[] args) {
        new Frog().hop();
      }
    }
    ```
    - ```
      Exception in thread "main" java.lang.NullPointerException:
        Cannot invoke "String.toLowerCase()" 
          because "Frog.name" is null
      ```
      - On instance and `static` variables, the JVM will tell you the name
      of the variable in the nice, easy-to-read format as we just saw.
      On local variables (including method parameters), it is not as
      friendly.
  - ```
    public class Frog {
      public void hop(String name) {
        System.out.print(name.toLowerCase());
      }
      public static void main(String[] args) {
        new Frog().hop(null);
      }
    }
    ```
    - ```
      Exception in thread "main" java.lang.NullPointerException:
        Cannot invoke "String.toLowerCase()" because "<parameter1>" is null
      ```
      - On method parameters it prints `<parameterX>`, while on 
      local variables it prints `<localX>`, where `X` is the order in 
      which the variable appears in the method. The reason for 
      this is that the name of the variable is lost when the code 
      is compiled.
    - If the class is compiled as:
        ```
        javac -g:vars Frog.java
        java Frog
        ```
        - ```js
          Exception in thread "main" java.lang.NullPointerException:
            Cannot invoke "String.toLowerCase()" because "name" is null
          ```
- **`IllegalArgument­Exception`**
_Thrown by programmer_ to indicate that method
has been passed illegal or inappropriate argument.
  - ```
    setNumberEggs(-­2):
    public void setNumberEggs(int numberEggs) {
      if (numberEggs < 0)
        throw new IllegalArgumentException("# eggs must not be negative");
        this.numberEggs = numberEggs;
    }
    ```
    - ```
      Exception in thread "main"
      java.lang.IllegalArgumentException: # eggs must not be negative
      ```
- **`NumberFormatException`**
_Subclass_ of `IllegalArgumentException`.
Thrown when attempt is made to convert `String` 
to numeric type but `String` doesn’t have 
appropriate format.
  - ```js
    Integer.parseInt("abc");
    ```
    The output looks like this:
    ```js
    Exception in thread "main"
    java.lang.NumberFormatException: For input string: "abc"
    ```
    - `NumberFormatException` is a subclass of `IllegalArgumentException`.
# **Checked Exception Classes**
- Checked exceptions have `Exception` in their hierarchy but
 not `RuntimeException`. They must be handled or declared.
  - **`FileNotFoundException`** and **`NotSerializableException`** 
  are subclasses of **`IOException`**.
- **`IOException`**
  - Thrown programmatically when problem reading or writing file.
- **`FileNotFoundException`**
  - Subclass of **`IOException`**. Thrown programmatically
when code tries to reference file that does not exist.
- **`NotSerializableException`**
  - Subclass of **`IOException`**. Thrown programmatically when 
  attempting to serialize or deserialize non serializable class.
- **`ParseException`**
  - Indicates problem parsing input.
- **`SQLException`**
  - Thrown when error related to accessing database.
# **Error Classes**
- Errors are unchecked exceptions that extend the `Error` class. They 
are thrown by the JVM and should not be handled or declared
- **`ExceptionInInitializerError`**
  - Thrown when `static` initializer throws exception and
doesn’t handle it
- **`StackOverflowError`**
  - Thrown when method calls itself too many times (called infinite 
  recursion because method typically calls itself without end)
- **`NoClassDefFoundError`**
  - Thrown when class that code uses is available 
  at compile time but not runtime
- These errors are unchecked and the code is often unable to recover from them