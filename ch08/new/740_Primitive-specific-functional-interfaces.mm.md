---

markmap:
  initialExpandLevel: 1
---

# **Primitive-specific<br/>functional interfaces**
- ```java
  ToXFunction<T> - x applyAsX(T)
  ```  
  - ```
    ToIntFunction<T>
    ```
    - ```
      int applyAsInt(T)
      ```
  - ```
    ToLongFunction<T>
    ```
    - ```
      long applyAsLong(T)
      ```
  - ```
    ToDoubleFunction<T>
    ```
    - ```
      double applayAsDouble(T)
      ```
- ```java
  ToXBiFunction<T,U> - x applyAsX(T,U)
  ```
  - ```
    ToIntBiFunction<T,U>
    ```
    - ```
      int applyAsInt(T,U)
      ```
  - ```
    ToLongBiFunction<T,U>
    ```
    - `long applyAsLong(T,U)`
  - ```
    ToDoubleBiFunction<T,U>
    ```
    - ```
      double applyAsDouble(T,U)
- ```java
  XToYFunction - y applyAsY(x)
  ```
  - ```
    DoubleToIntFunction
    ```
    - ```
      int applyAsInt(double)
      ```
  - ```
    DoubleToLongFunction
    ```
    - ```
      long applyAsLong(double)
      ```
  - ```
    IntToDoubleFunction
    ```
      - ```
        double applyAsDouble(int)
        ```
  - ```
    IntToLongFunction
    ```
    - ```
      long applyAsLong(int)
      ```
  - ```
    LongToDoubleFunction
    ```
    - ```
      double applyAsDouble(long)
      ```
  - ```
    LongToIntFunction
    ```
    - ```
      int applyAsInt(long)
      ```
- ```java
  ObjXConsumer<T> - void accept(T,x)
  ```
  - ```
    ObjIntConsumer<T>
    ```
    - ```
      void accept(T,int)
      ```
  - ```
    ObjLongConsumer<T>
    ```
    - ```
      void accept(T,long)
      ```
  - ```
    ObjDoubleConsume<T>
    ```
    - ```
      void accept(T,double)
      ``` 
- **1.** Generics are gone from some of the interfaces, and instead the type name tells us what
primitive type is involved. In other cases, such as `IntFunction`, only the return type
generic is needed because we’re converting a primitive int into an object.
**2.** The single abstract method is often renamed when a primitive type is returned.
- Which functional interface would you use to fill in the blank to make the following code compile?
  ```java
  var d = 1.0;
  ______________ f1 = x->1;
  f1.applyAsInt(d);`
  ```
  When you see a question like this, look for clues. You can see that the functional interface in 
  question takes a `double` parameter and returns an `int`. You can also see that it has a single 
  abstract method named `applyAsInt`. The `DoubleToIntFunction` and `ToIntFunction` functional 
  interfaces meet all three of those criteria.