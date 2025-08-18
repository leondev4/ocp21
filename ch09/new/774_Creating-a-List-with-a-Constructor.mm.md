---

markmap:
  initialExpandLevel: 1
---
# **Creating a `List` with a Constructor**
- Most `Collections` have two constructors. The following shows 
them for `LinkedList`:
  ```java
  var linked1 = new LinkedList<String>();
  var linked2 = new LinkedList<String>(linked1);
  ```
  The first says to create an empty `LinkedList` containing all 
  the defaults. The second tells Java that we want to make a 
  copy of another `LinkedList` (`Collection`).
- ```java
  ArrayList
  ```
  - ```java
    var list1 = new ArrayList<String>();
    ```
    - Constructs an empty `ArrayList` with an initial capacity of ten.
  - ```java
    var list2 = new ArrayList<String>(list1);
    ```
  - ```java
    var list3 = new ArrayList<String>(10);
    ```
    - Constructs an empty `ArrayList` with the specified initial capacity.
- Using `var` with
 `ArrayList`
  - ```java
    var strings = new ArrayList<String>();
    strings.add("a");
    for (String s: strings) {}
    ```
  - ```
    var list = new ArrayList<>();
    ```
    - The type of the `list` is `ArrayList<Object>`
  - ```java
    var list = new ArrayList<>();
    list.add("a");
    for (String s: list) { } // DOES NOT COMPILE
    ```
    - In the loop, we need to use the 
    `Object` type rather than `String`