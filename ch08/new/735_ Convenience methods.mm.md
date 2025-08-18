---

markmap:
  initialExpandLevel: 1
---
# **Convenience default<br/>methods**
- `Consumer`
  - ```java
    public default Consumer andThen(Consumer)
    ```
    - `c1.andThen(c2)` first executes `c1` and then`c2`; both use the same input
- `Function`
  - ```java
    public default Function andThen(Function)
    ```
    - `f1.andThen(f2)` first executes `f1` and the output is input of `f2`
  - ```java
    public default Function compose(Function)
    ```
    - `f1.compose(f2)` first executes `f2` and the outputs is input of `f1`
- `Predicate`
  - ```java
    public default Predicate and(Predicate)
    ```
  - ```java
    public default Predicate or(Predicate)
    ```
  - ```java
    public default Predicate negate()
    ```