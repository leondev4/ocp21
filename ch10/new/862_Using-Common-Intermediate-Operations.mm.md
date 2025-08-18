---

markmap:
  initialExpandLevel: 1
---
# **Using Common Intermediate Operations**
- Intermediate operation produces a stream as its result
- **Filtering**
The `filter()` method returns a `Stream` with 
elements that match a given expression.
  - ```js
    public Stream<T> filter(Predicate<? super T> predicate)
    ```
    - ```js
      Stream<String> s = Stream.of("alex", "gloria", "marty");
      s.filter(n -> n.startsWith("a"))
       .forEach(System.out::println); //alex
      ```
- **Removing Duplicates**
The `distinct()` method returns a stream with duplicate
values removed.  Java calls `equals()` to determine 
whether the objects are equivalent. 
  - ```
    public Stream<T> distinct()
    ```
    - ```js
      Stream<String> s = Stream.of("alex", "marty","alex");
      s.distinct()
       .forEach(System.out::print);//alexmarty
       ```
- **Restricting by Position**
The `limit()` and `skip()` methods can make a `Stream` 
smaller, or `limit()` could make a finite stream out of 
an infinite stream
  - ```
    public Stream<T> limit(long maxSize)
    public Stream<T> skip(long n)
    ```
    - ```js
      Stream<Integer> s = Stream.iterate(1, n->n+2);
      s.skip(3)
       .limit(2)
       .forEach(System.out::print);//79
       ```
- **Mapping**
The `map()` method creates a one-­to-­one mapping from the
elements in the stream to the elements of the next step in
the stream. 
  - ```js
    public <R> Stream<R> map(Function<? super T, ? extends R> mapper)
    ```
    - ```js
      Stream<String> s = Stream.of("alex", "gloria", "marty");
      s.map(String::length) //n->n.length()
       .forEach(System.out::print);//465
      ```
      Turns a `String` into an `Integer`.
- **flatMap**
This is helpful when you want to remove empty elements 
from a stream or combine a stream of lists. 
  - ```js
    public <R> Stream<R> flatMap( Function<? super T, 
      ? extends Stream<? extends R>> mapper)
    ```
    - ```js
        List<String> zero = List.of();
        var one = List.of("Bonobo");
        var two = List.of("Mama Gorilla", "Baby Gorilla");

        Stream<List<String>> animals = Stream.of(zero, one, two);
        Stream<String> streamListAnimals = animals.flatMap(
            m -> m.stream());
        streamListAnimals.forEach(System.out::println);
        ```
        -   Here’s the output:
            ```js
            Bonobo
            Mama Gorilla
            Baby Gorilla
            ```
- **Concatenating Streams**
  - ```
    var one = Stream.of("Bonobo");
    var two = Stream.of("Mama Gorilla", "Baby Gorilla");
    Stream.concat(one, two)
          .forEach(System.out::println);
    ```
- **Sorting**
The `sorted()` method returns a stream with the elements
 sorted. Java uses natural ordering unless we specify a
  `comparator`.
  - ```js
    public Stream<T> sorted()
    public Stream<T> sorted(Comparator<? super T> comparator)
    ```
    - ```js
      Stream<String> s = Stream.of("brown-­", "bear-­");
      s.sorted()
       .forEach(System.out::print); // bear-­brown-­`
    - ```js
      Stream<String> s = Stream.of("brown bear-­", "grizzly-­");
      s.sorted(Comparator.reverseOrder())
       .forEach(System.out::print); // grizzly-­brown bear-­
       ```
- **Taking a Peek**
The `peek()` is useful for debugging because it allows us to
perform a stream operation without changing the stream.
  - ```js
    public Stream<T> peek(Consumer<? super T> action)
    ```
    - ```js
      var stream = Stream.of("black bear","brown bear","grizzly");
      long count = stream.filter(s -> s.startsWith("g"))
        .peek(System.out::println).count();      // grizzly
      System.out.println(count);                 // 1
      ```