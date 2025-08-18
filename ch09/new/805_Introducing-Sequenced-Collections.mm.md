---

markmap:
  initialExpandLevel: 1
---
# **Introducing Sequenced Collections**
- New to Java 21 are sequenced collections, 
  which includes  three interfaces
  - ```
    1. SequencedCollection
    2. SequencedSet
    3. SequencedMap
    ```
    - A sequenced collection is a collection in which the encounter order is
    well-defined. By encounter order, it means all of the elements can be
    read in a repeatable way. While the elements of the collection may be
    sorted, it is not required.
      - #### Working with `SequencedCollection`
        An `ArrayList` is a `SequencedCollection`, since its first and last elements 
        are well-defined, as is the order of all intermediate elements.
        - **TABLE 9.11** `SequencedCollection` Methods
          |Method|Description|
          |-|-|
          |`addFirst(E e)` <br/>`addLast(E e)`|Adds element as the first/last element in the collection|
          |`getFirst()`<br/>`getLast()`|Retrieves the first/last element in the collection|
          |`removeFirst()`<br/>`removeLast()`|Removes the first/last element in the collection|
          |`reversed()`|Returns a reverse-ordered **view** of the collection|
          - For example, let’s say we have the following method that welcomes the next 
          visitor to the zoo:
            ```java
            public void welcomeNext(SequencedCollection<String> visitors) {
              System.out.println("Welcome to the Zoo! " + visitors.getFirst());
              visitors.removeFirst();
            }
            ```
            - We can now apply various sequenced collections to this method:
              ```java
              var visitArrayList = new ArrayList<String>(List.of("Huey", "Dewey","Louie"));
              var visitLinkedList = new LinkedList<String>(List.of("Moe", "Larry", "Shemp"));
              var visitTreeSet = new TreeSet<String>(Set.of("Alvin", "Simon", "Theodore"));
              welcomeNext(visitArrayList); // Welcome to the Zoo! Huey - [Dewey, Louie]
              welcomeNext(visitLinkedList); // Welcome to the Zoo! Moe - [Larry, Shemp]
              welcomeNext(visitTreeSet); // Welcome to the Zoo! Alvin  - [Simon, Theodore]
              ```
- Sequenced collections grant us the ability to work with lots of different
types that all have a well-defined encounter order. Using some of the 
other methods from **Table 9.11**, we can even rearrange the elements.
  ```java
  public void moveToEnd(SequencedCollection<String> visitors) {
   visitors.addLast(visitors.removeFirst());
  }
  ```
  - What happens if we call this new method on another group of collections?
    ```java
    var visitArrayList = new ArrayList<String>(List.of("Bluey", "Bingo", "Socks"));
    var visitLinkedList = new LinkedList<String>(List.of("Garfield", "Odie"));
    var visitTreeSet = new TreeSet<String>(Set.of("Tom", "Jerry"));

    moveToEnd(visitArrayList);
    welcomeNext(visitArrayList); // Welcome to the Zoo! Bingo

    moveToEnd(visitLinkedList);
    welcomeNext(visitLinkedList); // Welcome to the Zoo! Odie

    moveToEnd(visitTreeSet);      // UnsupportedOperationException
    ```
    - Just because a method implements `SequencedCollection` doesn’t 
    mean the class supports all of the methods in **Table 9.11**. In this 
    example, the `addLast()` call fails at runtime because you can’t 
    insert an item at the end of a sorted structure. Doing so could 
    violate the comparator within the `TreeSet`. 
    `TreeSet` does not support `addFirst()` and `addLast()` methods.
      - A `SequencedSet` is a subtype of `SequencedCollection` and adds `SequencedSet<T> reversed()` 
      method; therefore, it inherits all its methods. It only applies to `SequencedCollection` 
      classes that also implement `Set`, such as `LinkedHashSet` and `TreeSet`.
        - **Working with `SequencedMap`**
          A `SequencedMap` is a `Map` with a defined encounter order. 
          We define common methods in **Table 9.12**.
          - **TABLE 9.12** Common `SequencedMap` Methods
            |Method|Description|
            |-|-|
            |`firstEntry()`<br/>`lastEntry()`|Retrieves the first/last key-value pair in the map|
            |`pollFirstEntry()`<br/>`pollLastEntry()`|Removes and retrieves the first/last key-value pair in the map|
            |`putFirst(K k, V v)`<br/>`putLast(K k, V v)`|Adds the key-value pair as the first/last element in the map|
            |`reversed()`|Returns a reverse-ordered view of the map|
- Let’s define a method for working with `SequencedMap`.
  ```java
  public void welcomeNext(SequencedMap<String, String> visitors) {
    System.out.println("Welcome to the Zoo! " + visitors.
                pollFirstEntry());
  }
  ```
  - What do you think the following snippet prints?
    ```java
    var visitHashMap = new HashMap<String,String>(
      Map.of("1", "Yakko", "2", "Wakko", "3", "Dot"));
    welcomeNext(visitHashMap);
    ```
    - Trick question! It actually doesn’t compile. Like we explained with
    `HashSet` earlier, a `HashMap` does not have an ordering, so it cannot 
    be used as a `SequencedMap`. What about this example?
      ```java
      var visitTreeMap = new TreeMap<String,String>(
        Map.of("Pink", "Blossom", "Green", "Buttercup", "Blue",
      "Bubbles"));
      welcomeNext(visitTreeMap);
      ```
      - If you guessed `Welcome to the Zoo! Blue=Bubbles`, then you were paying
      attention when we covered `TreeMap`. A `TreeMap` sorts things by the natural 
      order of its keys, not the order in which they were added to the map. Since 
      `Blue` is the first key in sorted order, it is the first pair printed.
      **`HashSet` and `HashMap` are not sequenced.**
        - **Using Unmodifiable Wrapper Views**
          An unmodifiable view is a wrapper object around a collection that
          cannot be modified through the view itself. While the view object
          cannot be modified, the underlying data can still be modified.
- There are four methods you should be familiar with for the exam 
that create unmodifiable views of a collection:
  ```java
  Collection<String> coll =
    Collections.unmodifiableCollection(List.of("brown"));
  List<String> list =
    Collections.unmodifiableList(List.of("orange"));
  Set<String> set =
    Collections.unmodifiableSet(Set.of("green"));
  Map<String,Integer> map = 
    Collections.unmodifiableMap(Map.of("red",1));
  ```
  - Let’s consider some code that uses them:
    ```java
    10: Map<String, Integer> map = new TreeMap<>();
    11: map.put("blue", 41);
    12: map.put("red", 90);
    13: List<String> list = Arrays.asList("green", "yellow");
    14: Set<String> set = new HashSet<>(list);// creates a copy of list
    15:
    16: Map<String, Integer> mapView = Collections.unmodifiableMap(map);
    18: Collection<String> collView = Collections.unmodifiableCollection(list);
    19: List<String> listView = Collections.unmodifiableList(list);
    20: Set<String> setView = Collections.unmodifiableSet(set);
    ```
    - As you might expect, trying to modify an unmodifiable view throws an 
    exception. When run independently, each of the following compiles 
    and throws an `UnsupportedOperationException` at runtime.
      ```
      collView.add("pink");
      setView.remove("green");
      mapView.put("blue", 42);
      ```
      - However, since it is a view, nothing prevents you from changing 
      the original values. For example:
        ```java
        24: System.out.println(mapView);  // {blue=41, red=90}
        25: System.out.println(collView); // [green, yellow]
        26: System.out.println(listView); // [green, yellow]
        27: System.out.println(setView);  // [green, yellow]
        28:
        29: map.put("blue", 105);
        30: list.set(1, "purple");
        31:
        32: System.out.println(mapView);  // {blue=105, red=90}
        33: System.out.println(collView); // [green, purple]
        34: System.out.println(listView); // [green, purple]
        35: System.out.println(setView);  // [green, yellow]
        ```
        - On line 29, notice that the value of `blue` is changed to `105` in the original 
        `TreeMap` and it shows up as changed in `mapView` on line 32. The `list`
        variable created on line 13 refers to a fixed sized backed array. Which means 
        both `collView` and `listView` represent a view of a `List` that refers 
        to a backed array. Since the value is set on line 30, it remains the same 
        size, and the change properly shows up in our views.
        However, `setView` has not changed value. The constructor on line 14
        makes a new set that is disconnected from the original data structure.
        This means line 30 has no effect on `set`.
          - What happens if we try adding elements to these collections?
            ```java
            36: set.add("orange");
            37: System.out.println(setView); // [green, yellow, orange]
            38:
            39: list.add("orange"); // UnsupportedOperationException
            ```
            - Line 36 successfully modifies the underlying `HashSet`, with the changes
            reflected in the view on line 37. Line 39 throws an exception at runtime. 
            Remember, the `list` was created with `Arrays.asList()`. As we
            saw earlier in the chapter, you can replace/sort elements in such objects
            but you cannot add/remove elements. For the exam, remember to
            check the type of the underlying object to determine if things can be
            added, removed, or modified.
- **TABLE 9.13** Java Collections Framework types
  |Type|Can contain <br/>duplicate elements?|Elements always<br/> ordered?|Has keys <br/>and values?|Must add/remove <br/>in specific order?|
  |-|-|-|-|-|
  |`List`|Yes|Yes (by index)|No|No|
  |`Queue`|Yes|Yes (retrieved in<br/> defined order)|No|Yes|
  |`Set`|No|No|No|No|
  |`Map`|Yes (for values)|No|Yes|No|
  - **TABLE 9.14** Collection classes
    |Type|Java Collections<br/>Framework interfaces|Ordered?|Sorted?|Calls<br/>`hashCode`?|Calls<br/>`compareTo`?|
    |-|-|-|-|-|-|
    |`ArrayDeque`|`Deque`<br/>`SequencedCollection`|Yes|No|No|No|
    |`ArrayList`|`List`<br/>`SequencedCollection`|yes|No|No|No|
    |`HashMap`|`Map`|No|No|Yes|No|
    |`HashSet`|`Set`|No|No|Yes|No|
    |`LinkedList`|`Deque`<br/>`List`<br/>`SequencedCollection`|Yes|No|No|No|
    |`LinkedHashSet`|`Set`<br/>`SequencedCollection`<br/>`SequencedSet`|Yes|No|Yes|No|
    |`LinkedHashMap`|`Map`<br/>`SequencedMap`|Yes|No|Yes|No|
    |`TreeMap`|`Map`<br/>`SequencedMap`|Yes|Yes|No|Yes|
    |`TreeSet`|`Set`<br/>`SequencedCollection`<br/>`SequencedSet`|Yes|Yes|Not|Yes|
    - For sorted sets (`TreeSet`), `null` is not permitted; and for sorted maps 
    (`TreeMap`), `null` keys are not permitted.