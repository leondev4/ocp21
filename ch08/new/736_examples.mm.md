---

markmap:
  initialExpandLevel: 1
---
# **examples**
- `Predicate`
  - Let’s start with these two `Predicate` variables:

    ```java
    Predicate<String> egg = s->s.contains("egg");
    Predicate<String> brown = s->s.contains("brown");
    ```
      <br/>

      Now we want a `Predicate` for brown eggs and another for all other colors of eggs.  
      A way to deal with this situation is to use two of the default methods on `Predicate`.
      <br/>
      ```java
      Predicate<String> brownEggs = egg.and(brown);`
      Predicate<String> otherEggs = egg.and(brown.negate());
      ```
- `Consumer`
  - The `andThen(Consumer)` method, which runs two functional 
  interfaces in sequence:

    ```java
    Consumer<String> c1 = x->System.out.print("1: " + x);
    Consumer<String> c2 = x->System.out.print(",2: " + x);

    Consumer<String> combined = c1.andThen(c2);
    combined.accept("Annie"); // 1: Annie,2: Annie
    ```
    - Notice how the same parameter is passed to both `c1` 
    and `c2`. This shows that the `Consumer` instances 
    are run in sequence and are independent of each other.
    - `andThen` runs from left to right, does not modify the parameter
- `Function`
  - ```java
    Function<Integer, Integer> f1 = x -> x + 1;
    Function<Integer, Integer> f2 = x -> x * 2;
    Function<Integer, Integer> combined = f2.andThen(f1);
    System.out.println(combined.apply(3));// 7
    ```
    - `andThen` runs from left to right, first executing `f2` (`3*2=6` ) 
    and its output is used as input of `f1` (`6+1=7`)
  - ```java
    Function<Integer, Integer> f1 = x -> x + 1;
    Function<Integer, Integer> f2 = x -> x * 2;
    Function<Integer, Integer> combined = f2.compose(f1);
    System.out.println(combined.apply(3));// 8
    ```
    - `f1.compose(f2)` runs from the inside out and the output 
    of one is the input of the other
    - `f1` runs first, turning the `3` into `4`. Then the `f2` 
    runs, doubling the `4` to `8`. 