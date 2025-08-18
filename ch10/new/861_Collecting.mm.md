---

markmap:
  initialExpandLevel: 1
---
# **Collecting**
- Special type of reduction called a _mutable reduction_. It is more efficient than a 
regular reduction because we use the same mutable object while accumulating
  - Common mutable objects: `StringBuilder` and `ArrayList`
- ```js
  public <R> R collect(Supplier<R> supplier,
    BiConsumer<R, ? super T> accumulator ,//different type
    BiConsumer<R, R> combiner) //same type
  ```
  - ```js
    Stream<String> stream = Stream.of("w", "o", "l", "f");
    StringBuilder word = stream.collect(
      StringBuilder::new,
      StringBuilder::append,
      StringBuilder::append);
    System.out.println(word); // wolf
    ```
    - _supplier_, creates the object that will store the results as we collect data.
_accumulator_, is a `BiConsumer` that takes two parameters and doesn’t 
return anything. It is responsible for adding one more element to the 
data collection. In this example, it appends the next String to the
 `StringBuilder`. _combiner_, is another `BiConsumer.` It is responsible for 
 taking two data collections and merging them.
  - ```js
    Stream<String> stream = Stream.of("w", "o", "l", "f");
    TreeSet<String> set = stream.collect(
      TreeSet::new,
      TreeSet::add,
      TreeSet::addAll);
    System.out.println(set); // [f, l, o, w]
    ```
    - The collector has three parts as before. The supplier creates an empty `TreeSet`. 
    The accumulator adds a single `String` from the `Stream` to the `TreeSet`. The 
    combiner adds all of the elements of one `TreeSet` to another in case the 
    operations were done in parallel and need to be merged.
  -  This is useful when we are processing in parallel. 
- ```js
  public <R,A> R collect(Collector<? super T, A,R> collector)
  ```
  - ```js
    Stream<String> stream = Stream.of("w", "o", "l", "f");
    TreeSet<String> set =
      stream.collect(Collectors.toCollection(TreeSet::new));
    System.out.println(set); // [f, l, o, w]
    ```
  - If we didn’t need the set to be sorted, we could make the code 
  even shorter:
    ```js
    Stream<String> stream = Stream.of("w", "o", "l", "f");
    Set<String> set = stream.collect(Collectors.toSet());
    System.out.println(set); // [f, w, l, o]
    ```
    You might get different output. 