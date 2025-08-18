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