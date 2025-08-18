---

markmap:
  initialExpandLevel: 1
---
# **Creating a `List` with a Factory**
- You get a `List` back but don’t know the type
  - ```java
    Arrays.asList(varargs)
    ```
    - Returns a fixed-size list backed by the specified array.
      - Only you can edit, replace or sort elements;
       changes are reflected in both list and array
    - ```java
      String[] array = new String[] {"a", "b", "c"}; 
      List<String> asList = Arrays.asList(array);
      ```
      - ```java
        array[0] = "z";
        ```
        - Both `[z, b, c]`
      - ```java
        asList.set(0, "x");
        ```
        - Both `[x, b, c]`
  - ```java
    List.of(varargs)
    ```
    - Returns immutable list
      - You can not edit, replace, add or remove elements
  - ```java
    List.copyOf(collection)
    ```
    - Returns immutable list with copy 
    of original collection’s values
      - You can not edit, replace, add or remove elements
- Let’s take a look at an example of these three methods:
  ```java
  16: String[] array = new String[] {"a", "b", "c"};
  17: List<String> asList = Arrays.asList(array); // [a, b, c]
  18: List<String> of = List.of(array);           // [a, b, c]
  19: List<String> copy = List.copyOf(asList);    // [a, b, c]
  20:
  21: array[0] = "z";
  22:
  23: System.out.println(asList); // [z, b, c]
  24: System.out.println(of);     // [a, b, c]
  25: System.out.println(copy);   // [a, b, c]
  26:
  27: asList.set(0, "x");
  28: System.out.println(Arrays.toString(array)); // [x, b, c]
  ```
  - When run independently, the following shows both types are
    immutable by throwing an exception when trying to set a value.
    ```java
    of.set(0, "y");   // UnsupportedOperationException
    copy.set(0, "y"); // UnsupportedOperationException
    ```
  - Similarly, each of the following lines throws an exception when
  adding or removing a value:
    ```java
    asList.add("z");  // UnsupportedOperationException
    of.remove(0);     // UnsupportedOperationException
    copy.remove(0);   // UnsupportedOperationException
    ```