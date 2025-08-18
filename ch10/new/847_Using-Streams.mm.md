---

markmap:
  initialExpandLevel: 2
---

- 
# **Using Streams**
- A _stream_ in Java is a sequence of data.
  - A _stream pipeline_ consists of the operations 
  that run on a stream to produce a result. 
- **Source:** Where the stream comes from.
  - **Intermediate operations:** Transforms the stream into another one. 
  There can be as few or as many intermediate operations as you’d 
  like. Since streams use lazy evaluation, _the intermediate operations 
  do not run until the terminal operation runs_.
    - **Terminal operation:** Produces a result. Since streams can 
    be used only once, the stream is no longer valid after a
    terminal operation completes.
- `Stream<T>` interface is defined in `java.util.stream` package
# **Creating Stream Sources**
- Creating Finite Streams
  - Empty
    - ```
      Stream.empty()
      ```
  - One element
    - ```
      Stream.of(T)
      ```
  - From varargs
    - ```
      Stream.of(T...)
      ```
  - From `Collection`
    - ```
      col.stream()
      ```
    - ```
      col.parallelStream()
      ```
- Creating Infinite Streams
  - Generate
    - ```js
      //generate(Supplier s)
      Stream.generate(Math::random)
      ```
  - Iterate
    - ```js
      //iterate(T seed, UnaryOperator<T> f)
      Stream.iterate(1, n->n+1)
      ```
      - Uses:
      seed
      `UnaryOperator`
    - ```js
      //iterate(T seed, Predicate p, UnaryOperator<T> f)
      Stream.iterate(1, n->n<5, n->n+1)
      ```
      - Uses:
        Seed
        `Predicate`
        `UnaryOperator`
        Stops if `Predicate` returns `false`