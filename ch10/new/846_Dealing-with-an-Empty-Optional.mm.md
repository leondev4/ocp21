---

markmap:
  initialExpandLevel: 1
---

# **Dealing with an Empty Optional**
 - Here’s how to code our average method:
    - ```js
      10: public static Optional<Double> average(int… scores) {
      11:   if (scores.length == 0) return Optional.empty();
      12:   int sum = 0;
      13:   for (int score: scores) sum += score;
      14:     return Optional.of((double) sum / scores.length);
      15: }
      ```
- ```js
  30: Optional<Double> opt = average();
  31: System.out.println(opt.orElse(Double.NaN));     //NaN
  32: System.out.println(opt.orElseGet(() -­> Math.random()));
  ```
  - Line 32 outputs Random double value like:
    `0.1375932295380165`
- ```
  30: Optional<Double> opt = average();
  31: System.out.println(opt.orElseThrow());
  ```
  - Line 31 throws `java.util.NoSuchElementException`
  - Without specifying a `Supplier` for the exception, Java will throw a
  `NoSuchElementException`
- ```js
  30: Optional<Double> opt = average();
  31: System.out.println(opt.orElseThrow(
  32:       () -­> new IllegalStateException()));
  ```
  - We can have the code throw a custom exception
if the `Optional` is empty.
  - **Wacht out:** we do not write `throw new IllegalStateException()` 
- ```js
  System.out.println(opt.orElseGet(
  () -­> new IllegalStateException())); // DOES NOT COMPILE
  ```
  - The `opt` variable is an `Optional<Double>`. This means the `Supplier` must return 
  a `Double`. Since this `Supplier` returns an exception, the type does not match.