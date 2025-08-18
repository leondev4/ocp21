---

markmap:
  initialExpandLevel: 1
---

# **convenience methods on `Collection`**
  - ```java
    public boolean add(E element)
    ```
    - Inserts a new element and returns 
    whether it was successful.
      - A `List` allows duplicates, making the return value `true`
       each time. A `Set` does not allow duplicates.
        - ```java
          3: Collection<String> list = new ArrayList<>();
          4: System.out.println(list.add("Sparrow")); // true
          5: System.out.println(list.add("Sparrow")); // true
          6:
          7: Collection<String> set = new HashSet<>();
          8: System.out.println(set.add("Sparrow")); // true
          9: System.out.println(set.add("Sparrow")); // false
          ```
  - ```java
    public boolean remove(Object object)
    ```
    - Removes a single matching value and 
    returns whether it was successful
      - It returns `false` if the element is not in `Collection`

        - ```java
          3: Collection<String> birds = new ArrayList<>();
          4: birds.add("hawk"); // [hawk]
          5: birds.add("hawk"); // [hawk, hawk]
          6: System.out.println(birds.remove("cardinal")); // false
          7: System.out.println(birds.remove("hawk")); // true
          8: System.out.println(birds); // [hawk]
          ```
  - ```java
    public boolean isEmpty()
    public int size()
    ```
    - `isEmpty()` and `size()` methods look at how
      many elements are in the `Collection`
      - ```java
        Collection<String> birds = new ArrayList<>();
        System.out.println(birds.isEmpty());  // true
        System.out.println(birds.size());     // 0
        birds.add("hawk");                    // [hawk]
        birds.add("hawk");                    // [hawk, hawk]
        System.out.println(birds.isEmpty());  // false
        System.out.println(birds.size());     // 2
        ```
  - ```java
    public void clear()
    ```
    - Provides an easy way to discard 
    all elements of the `Collection`
      - After calling `clear()`, `Collection` is back to being empty of size 0.
        - ```java
          Collection<String> birds = new ArrayList<>();
          birds.add("hawk");                    // [hawk]
          birds.add("hawk");                    // [hawk, hawk]
          System.out.println(birds.isEmpty());  // false
          System.out.println(birds.size());     // 2
          birds.clear();                        // []
          System.out.println(birds.isEmpty());  // true
          System.out.println(birds.size());     // 0
          ```
  - ```java
    public boolean contains(Object obj)
    ```
    - Checks whether a certain value is in the `Collection`
      - ```java
        Collection<String> birds = new ArrayList<>();
        birds.add("hawk");                            // [hawk]
        System.out.println(birds.contains("hawk"));   // true
        System.out.println(birds.contains("robin"));  // false
        ```
        The `contains()` method calls `equals()` on elements of the `ArrayList` 
        to see whether there are any matches.
- ```java
  public boolean removeIf(Predicate<? super E> filter)
  ```
  - Removes all elements that match a condition
    - ```java
      4: Collection<String> list = new ArrayList<>();
      5: list.add("Magician");
      6: list.add("Assistant");
      7: System.out.println(list); // [Magician, Assistant]
      8: list.removeIf(s -> s.startsWith("A"));
      9: System.out.println(list); // [Magician]
      ```
      - ```java
        11: Collection<String> set = new HashSet<>();
        12: set.add("Wand");
        13: set.add("");
        14: set.removeIf(String::isEmpty); // s -> s.isEmpty()
        15: System.out.println(set);       // [Wand]
        ```
- ```java
  public void forEach(Consumer<? super T> action)
  ```
  - You can call on a `Collection` instead of writing a loop
    - ```java
      Collection<String> cats = List.of("Annie", "Ripley");
      cats.forEach(System.out::println);
      cats.forEach(c -> System.out.println(c));
      ```
- ```java
  boolean equals(Object object)
  ```
  - There is a custom implementation of `equals(Object)` so you can 
  compare two `Collection`s to compare the type and contents. 
  `ArrayList` checks order while `HashSet` does not.
    - ```java
      List.of(1, 2).equals(List.of(2, 1));  //false
      Set.of(1, 2).equals(Set.of(2, 1));    //true
      List.of(1, 2).equals(Set.of(1, 2));   //false
      ```
- Java protects us from many problems with `Collections`. However, 
it is still possible to write a `NullPointerException`:
  - ```java
    3: var heights = new ArrayList<Integer>();
    4: heights.add(null);
    5: int h = heights.get(0); //NullPointerException
    ```