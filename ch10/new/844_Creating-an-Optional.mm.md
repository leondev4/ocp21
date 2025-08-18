---

markmap:
  initialExpandLevel: 1
---
# **Creating an Optional**
- ```
  Optional.empty()
  ```
  - It prints
    ```
    Optional.empty
    ```
- ```
  Optional.of(95.0)
  ```
  - It prints
    ```
    Optional[95.0]
    ```
- ```java
  Optional o = 
    Optional.ofNullable(value);
  ```
  - If `value` is `null`, `o` is assigned the empty 
  `Optional`. Otherwise, we wrap the value
- Did you notice that we use a `static` method to create an `Optional`? That’s
because `Optional` relies on a factory pattern and does not expose any 
`public` constructors.
- ```js
  Optional<Double> opt = average(90, 100);
  if (opt.isPresent())
    System.out.println(opt.get()); // 95.0
  ```
  - An `Optional` can take a generic type, making it easier to retrieve values from it.
  - You can see that `Optional<Double>` may or may not contain a value.
  - First we check whether the `Optional` contains a value. Then we print it out. 
  - What if we didn’t do the check, and the `Optional` was empty?
    ```js
    Optional<Double> opt = average();
    System.out.println(opt.get()); //NoSuchElementException
    ```
- That covers the `static` methods you need to know about `Optional`. **Table 10.1**
summarizes most of the instance methods on `Optional` that you need to know
for the exam.
  - **TABLE 10.1** Common `Optional<T>` instance methods
    |Method|When `Optional` is empty|When `Optional` contains value|
    |-|-|-|
    |`get()`|Throws exception|Returns value|
    |`ifPresent(Consumer c)`|Does nothing|Calls `Consumer` with value|
    |`isPresent()`|Returns `false`|Returns `true`|
    |`orElse(T other)`|Returns `other` parameter|Returns value|
    |`orElseGet(Supplier s)`|Returns result of calling `Supplier`|Returns value|
    |`orElseThrow()`|Throws `NoSuchElementException`|Returns value|
    |`orElseThrow(Supplier s)`|Throws exception created by calling `Supplier`|Returns value|




