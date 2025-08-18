---

markmap:
  initialExpandLevel: 1
---
# **Common functional interfaces for primitives**
- ```java
  XSupplier - x getAsX()
  ```
  - ```
    IntSupplier
    ```
    - ```
      int getAsInt()
      ```
  - ```
    LongSupplier
    ```
    - ```
      long getAsLong()
      ```
  - ```
    DoubleSupplier
    ```
    - ```
      double getAsDouble()
      ```
- ```java
  XConsumer - void accept(x)
  ```
  - ```
    IntConsumer
    ```
    - ```
      void accept(int)
      ```
  - ```
    LongConsumer
    ```
    - ```
      void accept(long)
      ```
  - ```
    DoubleConsumer
    ```
    - ```
      void accept(double)
      ```
- ```java
  XPredicate - boolean test(x)
  ```
  - ```
    IntPredicate
    ```
    - ```
      boolean test(int)
      ```
  - ```
    LongPredicate
    ```
    - ```
      boolean test(long)
      ```
  - ```
    DoublePredicate
    ```
    - ```
      boolean test(double)
      ```
- ```java
  XFunction<R> - R apply(x)
  ```
  - ```
    IntFunction<R>
    ```
    - ```
      R apply(int)
      ```
  - ```
    LongFunction<R>
    ```
    - ```
      R apply(long)
      ```
      - Solo el tipo de retorno generico es necesario
       porque se convierte un primitive a object.
  - ```
    DoubleFunction<R>
    ```
    - ```
      R apply(double)
      ```
- ```
  XBinaryOperator - x applyAsX(x,x)
  ```
  - ```
    IntBinaryOperator
    ```
    - ```
      int applyAsInt(int,int)
      ```
  - ```
    LongBinaryOperator
    ```
    - ```
      long applyAsLong(long,long)
      ```
  - ```
    DoubleBinaryOperator
    ```
    - ```
      double applyAsDouble(double,double)
      ```
- ```java
  XUnaryOperator - x applyAsX(x)
  ```
  - ```
    IntUnaryOperator
    ```
    - ```
      int applyAsInt(int)
      ```
  - ```
    LongUnaryOperator
    ```
    - ```
      long applyAsLong(long)
      ```
  - ```
    DoubleUnaryOperator
    ```
    - ```
      double applyAsDouble(double)
      ```
- No cambian el nombre del metodo, solo el tipo del parametro. `Consumer`, `Predicate` y `Function`