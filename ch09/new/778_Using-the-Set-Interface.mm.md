---

markmap:
  initialExpandLevel: 1
---
# **Using the `Set` Interface**
- You use a `Set` when you don’t want to allow duplicate entries.
  -  The main thing that all `Set` implementations have in 
  common is that they do not allow duplicates. 
- Classes that implement the `Set` interface:
  - A `HashSet` stores its elements in a hash table, which means the keys are a hash
and the values are an `Object`. This means the `HashSet` uses the `hashCode()`
method of the objects to retrieve them more efficiently.
    - _main benefit_
      - adding elements and checking whether an element is in the set
both have constant time
      - constant time for the basic operations (add, remove, contains and size),
  - A `LinkedHashSet` is basically a `HashSet` with an imaginary `LinkedList`
running across its elements. This allows you to iterate over the set in a
well-defined encounter order, which is often the order the elements
were inserted. That said, `LinkedHashSet` also includes methods to
add/remove elements from the front or back of the set, allowing you to
change the ordering as needed.
    - [Figure 9.4](https://1drv.ms/i/c/c83cfca51d5c2032/EaqIhgZFneVAtasQ1vH4G6IByBp0UFMSZdDgxaStlywaxQ?e=02ZMpJ) shows how you can envision these three classes being
stored.
  - A `TreeSet` stores its elements in a sorted tree structure. The main
   benefit is that the **set is always in sorted order.** The trade-off is
  that **adding or removing an element could take longer than with a
  `HashSet`**, especially as the tree grows larger.
    - The _main benefit_ is that the set is always in sorted order.
    - Adding and checking whether an element exists takes longer 
    than with a `HashSet`, especially as the tree grows larger.
    - Added objects must implement `Comparable` or pass a `Comparator` when
     creating the `TreeSet`. If not met it, `ClassCastException` is thrown
- Creating a `Set`with factory:
Like a `List`, you can create an immutable `Set`
  - ```java
    Set<Character> letters = Set.of('c', 'a', 't');
    ```
    - Make a copy of an existing one:
      ```java
      Set<Character> copy = Set.copyOf(letters);
      ```
- **Working with `Set` Methods**
  - ```java
    HashSet
    ```
    - Constructors (same to `LinkedHashSet`):
      ```java
      HashSet()
      HashSet(int initialCapacity)
      HashSet(Collection<? extends E> c)
      ```
      - ```java
        3: Set<Integer> set = new HashSet<>();
        4: boolean b1 = set.add(66);      // true
        5: boolean b2 = set.add(10);      // true
        6: boolean b3 = set.add(66);      // false
        7: boolean b4 = set.add(8);       // true
        8: set.forEach(x->System.out::print(x+","));//66,8,10
        ```
        - _Line 6_ returns `false`, because we already have 66 in the set, 
        and a set must preserve uniqueness. _Line 8_ prints the elements of
         the set in an _arbitrary_ order. In this case, it happens not to 
         be sorted order or the order in which we added the elements.
      
  - ```java
    LinkedHashSet
    ```
    - Hash table and linked list implementation of the `Set` interface. 
    This implementation differs from `HashSet` in that it maintains a
     doubly-linked list running through all of its entries. Its order is
      in  which the elements were inserted into the set. 
      - Let’s replace line 3 with a `LinkedHashSet` and see how the output
          changes.
          ```java
          3: Set<Integer> set = new LinkedHashSet<>();
          ```
          This time, the code prints the elements in the order they were
          inserted.
          `66,10,8,`
  - ```java
    TreeSet
    ```
    - Constructors:
      ```java
      TreeSet();//its elements must implement Comparable
      TreeSet(Collection<? extends E> c).
      TreeSet(Comparator<? super E> comparator)
      ```
      - ```java
        3: Set<Integer> set = new TreeSet<>();
        4: boolean b1 = set.add(66); // true
        5: boolean b2 = set.add(10); // true
        6: boolean b3 = set.add(66); // false
        7: boolean b4 = set.add(8);  // true
        8: set.forEach(System.out::print);//81066
        ```
        - The elements are printed out in their natural sorted order.
         Number wrapper types implement the `Comparable` 
         interface in Java, which is used for sorting