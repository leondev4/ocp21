---

markmap:
  initialExpandLevel: 1
---
# **Understanding Suppressed Exceptions**
- We already know that resources are closed before any `catch` or `finally` 
blocks coded by the programmer are executed.
- ```js
  1: public class JammedTurkeyCage implements AutoCloseable {
  2:   public void close() throws IllegalStateException { //throws is not necessary
  3:     throw new IllegalStateException("Cage door does not close");
  4: }
  5:   public static void main(String[] args) {
  6:     try (JammedTurkeyCage t = new JammedTurkeyCage()) {
  7:       System.out.println("Put turkeys in");
  8:     } catch (IllegalStateException e) {
  9:       System.out.println("Caught: " + e.getMessage());
  10:    }
  11:  }
  12: }
  ```
  - The `close()` method is automatically called by try-­with-­resources. It throws an
  exception, which is caught by our `catch` block and prints the following:
    ```
    Put turkeys in
    Caught: Cage door does not close
    ```
- What happens if the `try` block also throws an exception? When multiple exceptions are thrown,
 **all but the first are called suppressed exceptions**. The idea is that Java treats the first exception 
 as the primary one and adds on any that come up while automatically closing.
- ```js
  5: public static void main(String[] args) {
  6:    try (JammedTurkeyCage t = new JammedTurkeyCage()) {
  7:        throw new IllegalStateException("Turkeys ran off");
  8:    } catch (IllegalStateException e) {
  9:        System.out.println("Caught: " + e.getMessage());
  10:       for (Throwable t: e.getSuppressed())
  11:         System.out.println("Suppressed: "+t.getMessage());
  12:    }
  13:  }
  ```
  - Line 7 throws the primary exception. At this point, the `try` clause ends, and Java automatically calls
  the `close()` method. Line 3 of `JammedTurkeyCage` throws an `IllegalStateException`, which is added
  as a suppressed exception. Then line 8 catches the primary exception. 
 
    The program prints the following:
    ```
    Caught: Turkeys ran off
    Suppressed: Cage door does not close
    ```
- `catch` block looks for matches on the primary exception
  - ```js
    3: public static void main(String[] args) {
    4:   try(JammedTurkeyCage t = new JammedTurkeyCage()){
    5:     throw new RuntimeException("Turkeys ran off");
    6:   }catch (IllegalStateException e){
    7:     System.out.println("caught: " + e.getMessage());
    8:  }
    9:}
    ```
    - ```
      Exception in thread "main" java.lang.RuntimeException: Turkeys ran off
        at JammedTurkeyCage.main(JammedTurkeyCage.java:7)
          Suppressed: java.lang.IllegalStateException:
            Cage door does not close
              at JammedTurkeyCage.close(JammedTurkeyCage.java:3)
              at JammedTurkeyCage.main(JammedTurkeyCage.java:8)
      ```

      Java remembers the suppressed exceptions that go with a primary exception even if 
      we don’t handle them in the code.
- Suppressed exceptions apply only to exceptions thrown in the `try` clause. 
If the `finally` block throws an exception, the suppressed exceptions are lost.
  - ```js
    3: public static void main(String[] args) {
    4:   try (JammedTurkeyCage t = new JammedTurkeyCage()) {
    5:     throw new IllegalStateException("Turkeys ran off");
    6:   } finally {
    7:     throw new RuntimeException("and we couldn't find them");
    8:   }
    9: }
    ```
    - ```
      Exception in thread "main" java.lang.RuntimeException:
        and we couldn't find them
          at JammedTurkeyCage.main(JammedTurkeyCage.java:9)
      ```