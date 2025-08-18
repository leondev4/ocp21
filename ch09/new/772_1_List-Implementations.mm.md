---

markmap:
  initialExpandLevel: 1
---

# **`List` Implementations**
- ```java
  ArrayList
  ```
  - Is like a resizable array. When elements are 
  added, it automatically grows.
    - _main benefit:_ you can look up any element in constant time
      - Adding or removing an element is slower than accessing an element
- ```java
  LinkedList
  ```
  - Implements both `List` and `Deque`. It has all the methods 
  of a `List`. It also has methods to facilitate adding or 
  removing from the beginning and/or end of the list.
    - _main benefits_ are that you can access, add to, and remove 
    from the beginning and end of the list in constant time.