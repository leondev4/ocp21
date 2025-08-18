# **`BooleanSupplier`**
- `BooleanSupplier` is a separate type.
  ```java
  public interface BooleanSupplier {
    boolean getAsBoolean();
  }
  ``` 
- It works just as you’ve come to expect from functional interfaces. 
- ```java
  12:   BooleanSupplier b1=()->true;
  13:   BooleanSupplier b2=()->Math.random() > .5;
  14:   System.out.println(b1.getAsBoolean()); // true
  15:   System.out.println(b2.getAsBoolean()); // false
  ```
- Lines 12 and 13 each create a `BooleanSupplier`, which is the only 
functional interface for `boolean`. Line 14 prints `true`, since it is
the result of `b1`. Line 15 prints `true` or `false`, depending on the
random value generated.