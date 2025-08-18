# **Common functional interfaces**
  - ```java
    Supplier<T>
    ```
    - ```java
      public T get()
      ```
  - ```java
    Consumer<T>
    ```
    - ```java
      public void accept(T)
      ```
  - ```java
    BiConsumer<T,U>
    ```
    - ```java
      public void accept(T,U)
      ```
  - ```java
    Predicate<T>
    ```
    - ```java
      public void test(T)
      ```
  - ```java
    BiPredicate<T,U>
    ```
    - ```java
      public void test(T,U)
      ```
  - ```java
    Function<T,R>
    ```
    - ```java
      public R apply(T)
      ```
    - ```java
      UnaryOperator<T>
      ```
      - ```java
        public T apply(T)
        ```
  - ```java
    BiFunction<T,U,R>
    ```
    - ```java
      public R apply(T,U)
      ```
    - ```java
      BinaryOperator<T>
      ```
      - ```java
        public T apply(T,T)
        ```