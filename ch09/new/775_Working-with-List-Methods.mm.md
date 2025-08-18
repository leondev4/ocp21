---

markmap:
  initialExpandLevel: 1
---
# **Working with `List` Methods**
- **index is invalid =** index is out of range (`index < 0 || index >= size()`)
- The methods in the `List` interface are for working with indexes
  - ```java
    boolean add(E element)
    ```
    - Adds element to end (available on all `Collection` APIs).
  - ```java
    public void add(int index, E element)
    ```
    - Adds element at index and moves the rest toward the end.
      `IndexOutOfBoundsException` - if  _index is invalid_.
  - ```java
    E get(int index)
    ```
    - Returns element at index.     
    `IndexOutOfBoundsException` - if _index is invalid_
  - ```java
    int indexOf(Object o)
    ```
    - Returns the index of the first matching element or `-1` if not found.
  - ```java
    int lastIndexOf(Object o)
    ```
    - Returns the index of the last matching element or `-1` if not found.
  - ```java
    E remove(int index)
    ```
    - Removes element at index and moves the rest
      Throws exception if _index is invalid_.
  - ```java 
    default void replaceAll(UnaryOperator<E> op)
    ```
    - Replaces each element in list with result of
operator.
  - ```java
    E set(int index, E e)
    ```
    - Replaces element at index and returns original.
    `IndexOutOfBoundsException` if _index is invalid_.
  - ```java
     default void sort(Comparator<? super E> c)
     ```
     - Sorts list.
  - Throws `UnsupportedOperationException` - if the list is unmodifiable
   (modification is not supported, `List.of` or `List.copyOf`)
- The following statements demonstrate most of these methods for 
  working  with a `List`:
  ```java
  3:  List<String> list = new ArrayList<>();
  4:  list.add("SD");                         // [SD]
  5:  list.add(0,"NY");                       // [NY,SD]
  6:  list.set(1, "FL");                      // [NY,FL]
  7:  System.out.println(list.**get**(0));        // NY
  8:  list.remove("NY");                      // [FL]
  9:  list.remove(0);                         // []
  10: list.set(0, "?");    // IndexOutOfBoundsException
  ```
  - ```java
    var numbers = Arrays.asList(1, 2, 3);
    numbers.replaceAll(x -> x*2);
    System.out.println(numbers);      // [2, 4, 6]
    ```
    - This lambda doubles the value of each element in the list.
- ```java
  boolean remove(object) //from Collection
  E remove(int);//from List
  31: var list = new LinkedList<Integer>();
  32: list.add(3);                              //[3]
  33: list.add(2);                              //[3,2]
  34: list.add(1);                              //[3,2,1]
  35: list.remove(2); //index = 1               //[3,2]
  36: list.remove(Integer.valueOf(2)); //obj //true [3]
  37: System.out.println(list);                 //[3]`
  ```
  - The `remove()` method that takes an element will return `false` if the element
  is not found. Contrast this with the `remove()` method that takes an `int`, which
  throws an exception if the element is not found:
    ```java
    var list = new LinkedList<Integer>();
    list.remove(Integer.valueOf(100)); // Returns false
    list.remove(100);                  // IndexOutOfBoundsException
    ```
    - **Searching a `List`**
      The List interface includes two methods for searching for elements,
      `indexOf()` and `lastIndexOf()`. They work similarly to the methods of the 
      same name in the `String` class:
      ```java
      var list = List.of("peacock", "chicken", "peacock", "turkey");
      
      System.out.println(list.indexOf("peacock"));      // 0
      System.out.println(list.lastIndexOf("peacock"));  // 2
      System.out.println(list.indexOf("penguin"));      // -1
      ```
- Converting from `List` to an `Array`
  - ```java
    13: List<String> list = new ArrayList<>();
    14: list.add("hawk");
    15: list.add("robin");
    16: Object[] objectArray = list.toArray();
    17: String[] stringArray = list.toArray(new String[0]);
    18: list.clear();
    19: System.out.println(objectArray.length);       // 2
    20: System.out.println(stringArray.length);       // 2
    ```
    - The array is simply a copy.