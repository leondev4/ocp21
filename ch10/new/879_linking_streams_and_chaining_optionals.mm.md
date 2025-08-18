---

markmap:
  initialExpandLevel: 2
---
# **Linking Streams to the Underlying Data**
- ```js
  25: var cats = new ArrayList<String>();
  26: cats.add("Annie");
  27: cats.add("Ripley");
  28: var stream = cats.stream();
  29: cats.add("KC");
  30: System.out.println(stream.count());//3
  ```
  -  Lines 25–27 create a `List` with two elements. Line 28 requests that a 
stream be created from that `List`. Remember that streams are lazily 
evaluated. This means the stream isn’t created on line 28. An object 
is created that knows where to look for the data when it is needed.
On line 29, the `List` gets a new element. On line 30, the stream 
pipeline sees three elements when it runs giving us that count.
# **Chaining Optionals**
- A few of the intermediate operations for streams are 
available for `Optional`, as shown in **Table 10.9**.
  - **TABLE 10.9** Advanced `Optional` instance methods
    |Method|When<br/>`Optional` is <br/>empty|When `Optional` contains value|
    |-|-|-|
    |`filter(Predicate p)`|Returns<br/>empty<br/>`Optional`|Returns `Optional` containing the element if it<br/>matches the `Predicate`, otherwise empty<br/> `Optional`|
    |`flatMap(Function f)`|Returns<br/>empty<br/>`Optional`|Returns `Optional` with `Function` applied to the<br/> element. Return type of `Function` must<br/> inherit `Optional`.|
    |`map(Function f)`|Returns<br/>empty<br/>`Optional`|Returns `Optional` with `Function` applied to the<br/> element|
- Suppose that you are given an `Optional<Integer>` and asked
 to print the value, but only if it is a three-­digit number.
  - ```js
    private static void threeDigit(Optional<Integer> optional) {
      optional.map(n -> "" + n)        // part 1
      .filter(s -> s.length() == 3)    // part 2
      .ifPresent(System.out::println); // part 3
    }
    ```
    - Empty `Optional`: It sees an empty `Optional` and has both `map()` and `filter()` 
    pass it through. Then `ifPresent()` sees an empty `Optional` and doesn’t call 
    the `Consumer` parameter.
    - `Optional.of(4)`: It maps the number `4` to `"4"`. The `filter()` then returns an 
    empty `Optional` since the filter doesn’t match, and `ifPresent()` doesn’t call 
    the `Consumer` parameter.
    - `Optional.of(123)`: It maps the number `123` to `"123"`. The `filter()` then returns 
    the same `Optional`, and `ifPresent()` now does call the `Consumer` parameter.
- Now suppose that we wanted to get an `Optional<Integer>` representing the
length of the `String` contained in another `Optional`. Easy enough:
  ```js
  String str = "Marty";
  Optional<String> o = Optional.of(str);
  Optional<Integer> r = o.map(String::length);
                        //.map(x->x.length());
  System.out.println(r); //Optional[5]
  ```
  - What if instead we had a helper method that takes a `String` and did the logic
    of calculating something for us? It would return `Optional<Integer>`, such as this:
      ```js
      public static Optional<Integer> calculator(String text) {
      // Calculation logic here
      }
      ```
      - Using `map` to call it doesn’t work. The solution is
       to call `flatMap()`, instead:
          ```
          Optional<Integer> result = optional
            .flatMap(ChainingOptionals::calculator);
          ```