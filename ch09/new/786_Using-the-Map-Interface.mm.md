---

markmap:
  initialExpandLevel: 1
---
# **Using the `Map` Interface**
- You use a `Map` when you want to identify values by a key.
  - All `Map` classes have in common is that they have keys and values,
    - Given that `Map` doesn’t extend `Collection`, more methods are 
    specified on the `Map` interface. Since there are both keys and 
    values, we need generic type parameters for both. The class 
    uses `K` for key and `V` for value.
- Create a `Map` from factory
  - ```java
    var map = Map.of("key1", "value1", "key2", "value2");
    ```
  - ```java
    Map.copyOf(map);
    ```
  - ```java
    Map.ofEntries(
      Map.entry("key1", "value1"),
      Map.entry("key2", "value2"));
    ```
- From **Figure 9.1**, `HashMap`, `LinkedHashMap`, and `TreeMap` 
are the three classes that implement the `Map` interface.
- A `HashMap` stores the keys in a hash table.
  - The _main benefit_ is that adding elements and retrieving the element 
  by key both have constant time. You lose the order in which you 
  inserted the elements.
   
  - Constructors:
    ```java
    HashMap()
    HashMap(int initialCapacity)
    HashMap(Map<? extends K,? extends V> m)
    ```
- Like `LinkedHashSet`, the `LinkedHashMap` supports iterating over the
      elements in a well-defined order. This is generally the insertion order,
      although it also includes methods to add/remove elements at the front
      or back of the map.
  - Constructors:
    ```java
    LinkedHashMap()
    LinkedHashMap(int initialCapacity)
    LinkedHashMap(int initialCapacity, float loadFactor)
    LinkedHashMap(int initialCapacity, float loadFactor, boolean accessOrder)
    LinkedHashMap(Map<? extends K,? extends V> m)
    ```
- A `TreeMap` stores the keys in a sorted tree structure.
  - The _main benefit_ is that the keys are always in sorted order. Like a `TreeSet`, adding
   and checking whether a key is present takes longer as the tree grows larger.
   - Constructors:
      ```java
      TreeMap(): //Its keys must implement Comparable
      TreeMap(Comparator<? super K> comparator)
      TreeMap(Map<? extends K,? extends V> m)
      ```