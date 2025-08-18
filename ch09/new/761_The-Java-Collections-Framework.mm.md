---
markmap:
  initialExpandLevel: 1
---
# **Using Common Collection APIs**
- A collection is a group of objects contained in a single object. The Java Collections Framework 
is a set of classes in `java.util` for storing collections. There are four main interfaces in the Java 
Collections Framework.
  - **`List`:** A list is an ordered collection of elements that allows duplicate 
  entries. Elements in a list can be accessed by an `int` index.
  - **`Set`:** A set is a collection that does not allow duplicate entries.
  - **`Queue`:** A queue is a collection that orders its elements in a specific 
  order for processing. A `Deque` is a subinterface of `Queue` that
  allows access at both ends.
  - **`Map`:** A map is a collection that maps keys to values, with no duplicate 
  keys allowed. The elements in a map are key/value pairs.
- [Figure 9.1](https://1drv.ms/i/c/c83cfca51d5c2032/ET55gG4FHyxBjNF6624NagkB_WVC25_O9ZtqIQG493w3Wg?e=WhLYRs) shows the `Collection` interface, its subinterfaces, and some classes that implement 
the interfaces that you should know for the exam. The interfaces are shown in rectangles, with 
the classes in rounded boxes.
- ### Understanding Generic Types
  - In Java, generics is just a way of saying parameterized type. For example, 
  a `List<Integer>` is a list of numbers, while `Set<String>` is a set of strings
    - Without generics, we’d have to write a lot of code like the following:
      ```java
      List numbers = new ArrayList(List.of(1,2,3));
      Integer element = (Integer)numbers.get(0); // Required cast to compile
      numbers.add("Welcome to the zoo!");        // Unrelated types allowed
      ```
    - With generics we can do better. The following change not only does
      away with the required cast from the previous code but also helps
      prevent unrelated objects from being added to the collection:
      ```java
      List<Integer> numbers = new ArrayList<Integer>(List.of(1,2,3));
      Integer element = numbers.get(0);   // No cast required
      numbers.add("Welcome to the zoo!"); // DOES NOT COMPILE
      ```
      - Getting a compiler error is good. You’ll know right away that something is wrong rather than 
      hoping to discover it later. Generics are convenient because the code for `List`, `Set`, and 
      other collections does not change based on the generic type. You can even use your own 
      class as the type, such as `List<Visitor>`.
- ### Shortening Generics Code
  - You have two declarations of generics:
    ```java
    List<Integer> list = new ArrayList<Integer>();
    Map<Long,List<Integer>> mapOfLists = new HashMap<Long,List<Integer>> ();
    ```
    - The diamond operator (`<>`) is a shorthand notation that allows you to
omit the generic type from the right side of a statement when the type
can be inferred. It is called the diamond operator because `<>` looks
like a diamond. Compare the previous declarations with these new,
much shorter versions:
      ```java
      List<Integer> list = new ArrayList<>();
      Map<Long,List<Integer>> mapOfLists = new HashMap<>();
      ```
    - The diamond operator cannot be used as the type in 
    a variable declaration. It can be used only on the right 
    side of an assignment operation.
      - ```java
        List<> list = new ArrayList<Integer>();// DOES NOT COMPILE
        ```
      - ```java
        void use(List<> data) {}// DOES NOT COMPILE
        ```
  - **Applying `var`**
    You can also use `var` to shorten expressions with generics.
      ```java
      var list = new ArrayList<Integer>();
      var mapOfLists = new HashMap<Long,List<Integer>>();
      ```
      Notice how the generic type is back on the right side. That’s because
      `var` infers the type from the right side of the declaration, whereas the
      diamond operator infers it from the left side.
      - **Using Both Shorteners**
        What happens if you use both var and the diamond operator?
        ```java
        var map = new HashMap<>();
        ```
      - Believe it or not, this does compile! If you try to have them both
      infer, there isn’t enough information and you get `Object` as the
      generic type. This is equivalent to the following:
        ```java
        HashMap<Object, Object> map = new HashMap<Object, Object>();
        ```