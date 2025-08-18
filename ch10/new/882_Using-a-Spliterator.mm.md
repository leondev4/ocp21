---

markmap:
  initialExpandLevel: 1
---
# **Using a `Spliterator`**
- Methods
  - ```
    Spliterator<T> trySplit()
    ```
    - Returns `Spliterator` containing ideally half of the data, which is removed 
    from current `Spliterator`. This method can be called multiple times and 
    will eventually return `null` when data is no longer splittable.
  - ```
    void forEachRemaining(Consumer<T> c)
    ```
    - Processes remaining elements in `Spliterator`.
  - ```
    boolean tryAdvance(Consumer<T> c)
    ```
    - Processes single element from `Spliterator` if any remain.
     Returns whether element was processed.
- ```js
  12: var list = List.of("bird-­", "bunny-­", "cat-­", "dog-­", "fish-­", "lamb-­","mouse-­");
  13: 
  14: Spliterator<String> originalBagOfFood = list.spliterator();
  15: Spliterator<String> emmasBag = originalBagOfFood.trySplit(); //takes 3 left
  16: emmasBag.forEachRemaining(System.out::print); //bird-­bunny-­cat-­
  17:
  18: Spliterator<String> jillsBag = originalBagOfFood.trySplit();//takes 2 left
  19: jillsBag.tryAdvance(System.out::print);// dog-­
  20: jillsBag.forEachRemaining(System.out::print); // fish-­
  21:
  22: originalBagOfFood.forEachRemaining(System.out::print); // lamb-mouse-­`
- ```js
  var originalBag = Stream.iterate(1, n -> ++n).spliterator();

  Spliterator<Integer> newBag = originalBag.trySplit();

  newBag.tryAdvance(System.out::print); // 1
  newBag.tryAdvance(System.out::print); // 2
  newBag.tryAdvance(System.out::print); // 3
  ```
  - You might have noticed that this is an infinite stream. No problem! The `Spliterator` recognizes 
  that the stream is infinite and doesn’t attempt to give you half. Instead, `newBag` contains a
  large number of elements. We get the first three since we call `tryAdvance()` three times. It 
  would be a bad idea to call `forEachRemaining()` on an infinite stream!