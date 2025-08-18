---

markmap:
  initialExpandLevel: 2
---

# **Overriding Methods with<brr> Checked Exceptions**
- It is only valid in checked exceptions
- Same class, Covariance class, or nothing in subclass methods
- ```
  class CanNotHopException extends Exception {}
  class Hopper {
    public void hop(){}
  }
  class Bunny extends Hopper {
    public void hop() throws CanNotHopException {} // DOES NOT COMPILE
  }
  ```
  - `hop()` isn’t allowed to throw any checked exceptions because the 
  `hop()`  method in the superclass `Hopper` doesn’t declare any.
- ```
  class Hopper {
    public void hop() throws CanNotHopException {}
  }
  class Bunny extends Hopper {
    public void hop() {} //This is fine, you can put nothing, CanNotHopException or subytpes
  }
  ```
- An overridden method not declaring one of the exceptions thrown by the parent method
is similar to the method declaring that it throws an exception it never actually throws. This
is perfectly legal.
# **Constructors with Checked Exceptions**
- It is only valid in checked exceptions
- Contravariance or nothing in parent class constructors
- ```
  class CanNotHopException extends Exception {}
  class Hopper {
    Hopper(){}
  }
  class Bunny extends Hopper {
      public Bunny() throws CanNotHopException{ super();}
  }
  class NewbornRabbit extends Bunny{
      //It must trows CanNotHopException or its parents
      public NewbornRabbit() throws Exception { super();}
  }
  ```
- In the `Bunny` constructor you can leave it alone or put `Exception` or subtypes; for example:
   ```js
  public Bunny() throws Exception{ super();}
  public Bunny() throws CanNotHopException{ super();}
  public Bunny(){ super();}
  ```
  They all compile if inserted independently.
# **Printing an Exception**
- ```js
  5: public static void main(String[] args) {
  6:  try {
  7:     hop();
  8:  } catch (Exception e) {
  9:    System.out.println(e + "\n");
  10:   System.out.println(e.getMessage()+ "\n");
  11:   e.printStackTrace();
  12:  }
  13: }
  14: private static void hop() {
  15:   throw new RuntimeException("cannot hop");
  16: }
  ```
  - **`java.lang.RuntimeException: cannot hop`**
  - **`cannot hop`**
  - ```
    java.lang.RuntimeException: cannot hop
      at Handling.hop(Handling.java:15)
      at Handling.main(Handling.java:7)
    ```