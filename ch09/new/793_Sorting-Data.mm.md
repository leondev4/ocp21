---

markmap:
  initialExpandLevel: 1
---

# **Sorting Data**
- Remember 7Up, when working with a `String`, numbers sort before 
letters, and uppercase letters sort before lowercase letters.
  - You can also sort objects that you create yourself. Java provides an interface called
`Comparable`. If your class implements `Comparable`, it can be used in data structures 
that require comparison. There is also an interface called `Comparator`, which is used 
to specify that you want to use a different order than the object itself provides.
    - In fact, you’ve seen many `Comparable` classes in this book include `String`,
    `StringBuilder`, `BigDecimal`, `BigInteger` and the primitive wrapper classes.
      - `Collections.sort(Collection<T>)`, returns `void`because the method 
      parameter is what gets sorted; it must implement `Comparable`, 
      if  does not, compile error occurs.
- #### Creating a `Comparable` Class
  - The `Comparable` interface has only one method.
    - ```
      public interface Comparable<T> {
        int compareTo(T o);
      }
      ```
      - The generic `T` lets you implement this method and specify the 
      type of your object. This lets you avoid a cast when implementing
       `compareTo()`. Any object can be `Comparable`.
        - For example, we have ducks and we want to sort them by name. First, 
        we create a `record` that implements `Comparable<Duck>`, and then 
        we implement the `compareTo()` method.
          ```
          public record Duck(String name) implements Comparable<Duck> {
            public int compareTo(Duck d) {
              return name.compareTo(d.name); // Sorts ascendingly by name
            }
          }
          ```
          - We can sort the ducks as follows:
            ```
            11: var ducks = new ArrayList<Duck>();
            12: ducks.add(new Duck("Quack"));
            13: ducks.add(new Duck("Puddles"));
            14: Collections.sort(ducks); // sort by name
            15: System.out.print(ducks); // [Duck[name=Puddles],Duck[name=Quack]]
            ```
            - If we didn’t implement the `Comparable` interface, all we have is a
            method named `compareTo()`, and line 14 would not compile.
            Finally, the `Duck` class implements `compareTo()`. Since `Duck` is 
            comparing objects of type `String` and the `String` class already has 
            a `compareTo()` method, it can just delegate.
- `compareTo` or 
`compare`returns
  - Negative number when the current object is smaller than the argument
  - zero (`0`)  when the current object is equivalent to the agument
  - Positive number when the current object is larger than the argument
- Let’s look at an implementation of `compareTo()` that compares numbers
 instead of `String` objects:
  ```
  2: public record ZooDuck(int id, String name) implements 
                              Comparable<ZooDuck> {
  3:   public int compareTo(ZooDuck d) {
  4:     return id - d.id; // Sorts ascendingly by id
  5:   }
  6: }
  ```
  - Line 4 shows one way to compare two `int` values. We could have used
  `Integer.compare(id, d.id)`, but we wanted to show you how to create
  your own. Be sure you can recognize both approaches. Remember that 
  `id - d.id` sorts in ascending order, and `d.id - id` sorts in descending 
  order.
    - Let’s try this new method out in some code:
      ```
      21: var d1 = new ZooDuck(5, "Daffy");
      22: var d2 = new ZooDuck(7, "Donald");
      23: System.out.println(d1.compareTo(d2)); // -2
      24: System.out.println(d1.compareTo(d1)); // 0
      25: System.out.println(d2.compareTo(d1)); // 2
      ```
      - Line 23 compares a smaller `id` to a larger one, and therefore it prints
      a negative number. Line 24 compares animals with the same `id`, and
      therefore it prints `0`. Line 25 compares a larger `id` to a smaller one,
      and therefore it returns a positive number.
        - **Casting the `compareTo()` Argument**
          ```
          public record LegacyDuck(String name) implements Comparable {
            public int compareTo(Object obj) {
              if(obj instanceof LegacyDuck d)
                return name.compareTo(d.name);
              throw new UnsupportedOperationException("Not a duck");
            } }
          ```
          Since we don’t specify a generic type for `Comparable`, Java assumes that
          we want an `Object`.
          - **Checking for `null`**
            ```java
            public record MissingDuck(String name) implements Comparable<MissingDuck> {
              public int compareTo(MissingDuck quack) {
                if (quack == null)
                  throw new IllegalArgumentException("Poorly formed duck!");
                if (this.name == null && quack.name == null)
                  return 0;
                else if (this.name == null) return -1;
                else if (quack.name == null) return 1;
                else return name.compareTo(quack.name);
              } }
            ```
            This method throws an exception if it is passed a `null` `MissingDuck` object. What about the 
            ordering? If the `name` of a duck is `null`, it’s sorted first.
            - **Keeping `compareTo()` and `equals()` Consistent**
              If you write a class that implements `Comparable`, you introduce new
              business logic for determining equality. The `compareTo()` method
              returns `0` if two objects are equal, while your `equals()` method returns
              `true` if two objects are equal. A natural ordering that uses `compareTo()`
              is said to be consistent with equals if, and only if, `x.equals(y)` is `true`
              whenever `x.compareTo(y)` equals `0`.
              Similarly, `x.equals(y)` must be `false` whenever `x.compareTo(y)` is not `0`.
- **Comparing Data with a `Comparator`**
Sometimes you want to sort an object that did not implement
`Comparable`, or you want to sort objects in different ways.
  ```
  public record Duck(String name, int weight) implements
                Comparable<Duck> {
    public int compareTo(Duck d) {
      return name.compareTo(d.name);
    }
    public String toString() { return name; }
  }
  ```
  - We now have the following:
    ```
    11: Comparator<Duck> byWeight = new Comparator<>() {
    12:   public int compare(Duck d1, Duck d2) {
    13:     return d1.weight() - d2.weight();
    14:   }
    15: };
    16: var ducks = new ArrayList<Duck>();
    17: ducks.add(new Duck("Quack", 7));
    18: ducks.add(new Duck("Puddles", 10));
    19: Collections.sort(ducks);
    20: System.out.println(ducks); // [Puddles, Quack]
    21: Collections.sort(ducks, byWeight);
    22: System.out.println(ducks); // [Quack, Puddles]
    ```
    - The `Duck` class itself can define only one `compareTo()` method. In this
    case, `name` was chosen. If we want to sort by something else, we have
    to define that sort order outside the `compareTo()` method using a
    separate class or lambda expression.
    Lines 11–15 show how to define a `Comparator` using an anonymous
    class. On lines 19–22, we sort with the class’s internal `Comparator` and
    then with the separate `Comparator` to see the difference in output.
      - `Comparator` is a functional interface. We can rewrite the `Comparator` on 
      lines 11–15 using a lambda expression, as shown here:
        ```
        Comparator<Duck> byWeight = (d1, d2) -> d1.weight()-d2.weight();
        ```
        Alternatively, we can use a method reference and a helper method to
        specify that we want to sort by weight.
        ```
        Comparator<Duck> byWeight = Comparator.comparing(Duck::weight);
        ```
        - `Comparator.comparing()` is a `static` interface method that creates a 
        `Comparator` given a lambda expression or method reference.
          - **Comparing `Comparable` and `Comparator`**
            **TABLE 9.8** Comparison of Comparable and Comparator
            |Difference|`Comparable`|`Comparator`|
            |-|-|-|
            |Package name|`java.lang`|`java.util`|
            |Interface must be implemented by class comparing?|Yes|No|
            |Method name in interface|`compareTo()`|`compare()`|
            |Number of parameters|1|2|
            |Common to declare using a lambda|No|Yes|
            - Do you see why this doesn’t compile?
              ```java
              var byWeight = new Comparator<Duck>() { // DOES NOT COMPILE
                public int compareTo(Duck d1, Duck d2) {
                  return d1.getWeight()-d2.getWeight();
                }
              };
              ```
              The method name is wrong. A `Comparator` must implement a method
              named `compare()`.
-  **Comparing Multiple Fields**
Sort by species name. If two squirrels are from the same  species, we want to 
sort the one that weighs the least first.
    ```
    Comparator<Squirrel> c = Comparator.comparing(Squirrel::getSpecies)
      .thenComparingInt(Squirrel::getWeight);
    ```
    - We can to sort in descending order by species.
      ```
      var c = Comparator.comparing(Squirrel::species).reversed();
      ```
      - **TABLE 9.9** Helper `static` methods for building a `Comparator`
        |Method|Description|
        |-|-|
        |`comparing(function)`|Compare by results of function that returns any `Object`<br/> (or primitive autoboxed into `Object`) .|
        |`comparingDouble(function)`|Compare by results of function that returns `double`|
        |`comparingInt(function)`|Compare by results of function that returns `int`.|
        |`comparingLong(function)`|Compare by results of function that returns `long`.|
        |`naturalOrder()`| Sort using order specified by the `Comparable`<br/> implementation on the object itself.|
        |`reverseOrder()`|Sort using reverse of order specified by `Comparable`<br/>implementation on the object itself.|
        - Methods that you can chain to a `Comparator` to further specify its behavior.
        **TABLE 9.10** Helper default methods for building a `Comparator`
          |Method|Description|
          |-|-|
          |`reversed()`|Reverse order of chained `Comparator`.|
          |`thenComparing(function)`|If previous `Comparator` returns `0`, use this comparator<br/>function that returns `Object` or can be autoboxed into one.<br/>Otherwise, return result from previous `Comparator`.|
          |`thenComparingDouble(function)`|If previous Comparator returns `0`, use this comparator<br/> function that returns double. Otherwise, return result from<br/> previous `Comparator`.|
          |`thenComparingInt(function)`|If previous `Comparator` returns `0`, use this comparator<br/>function that returns `int`. Otherwise, return result from<br/>previous `Comparator`.|
          |`thenComparingLong(function)`|If previous `Comparator` returns `0`, use this comparator<br/>function that returns `long`. Otherwise, return result from<br/>previous `Comparator`.|
- **Sorting and Searching**
The `Collections.sort()` method uses the `compareTo()` method to sort. 
It expects the objects to be sorted to be `Comparable`.
  ```java
  2: public class SortRabbits {
  3:   static record Rabbit(int id) {}
  4:   public static void main(String[] args) {
  5:     List<Rabbit> rabbits = new ArrayList<>();
  6:     rabbits.add(new Rabbit(3));
  7:     rabbits.add(new Rabbit(1));
  8:     Collections.sort(rabbits); // DOES NOT COMPILE
  9:    }
  10: }
  ```
  - Java knows that the `Rabbit` record is not `Comparable`. It knows sorting will fail, 
  so it doesn’t even let the code compile. You can fix this by passing a 
  `Comparator` to `sort()`. Remember that a `Comparator` is useful when you want 
  to specify sort order without using a `compareTo()` method.
    ```
    8:    Comparator<Rabbit> c = (r1, r2) -> r1.id - r2.id;
    9:    Collections.sort(rabbits, c);
    10:   System.out.println(rabbits); // [Rabbit[id=1], Rabbit[id=3]]
    ```
    - Suppose you want to sort the rabbits in descending order. You could
    change the `Comparator` to `r2.id - r1.id`. Alternatively, you could reverse
    the contents of the list afterward:
      ```
      8:  Comparator<Rabbit> c = (r1, r2) -> r1.id - r2.id;
      9:  Collections.sort(rabbits, c);
      10: Collections.reverse(rabbits);
      11: System.out.println(rabbits); //[Rabbit[id=3], Rabbit[id=1]]
      ```
      - The `Collections.reverse(collection)` method does not require that `collection` implement 
      `Comparable`; it only reverses the insertion order if the `collection` is not sorted.
        - The `sort()` and `binarySearch()` methods allow you to pass in a 
        `Comparator` object when you don’t want to use the natural order.
          - The `binarySearch()` method requires a sorted `List`. In ascending order 
            ```
            11: List<Integer> list = Arrays.asList(6,9,1,8);
            12: Collections.sort(list); // [1, 6, 8, 9]
            13: System.out.println(Collections.binarySearch(list, 6)); // 1
            14: System.out.println(Collections.binarySearch(list, 3)); // -­2
            ```
            The number `3` would need to be inserted at index `1` (after the number `1` 
            but before the number `6`). Adding `1` that gives us 2 and negating that 
            gives us `−2`.
            **Works like `~` operator:**
            ```
            int num = 10;
            num = ~num;
            System.out.println(num);//-11
            ```
            - What do you think the following outputs?
              ```
              3: var names = Arrays.asList("Fluffy", "Hoppy");
              4: Comparator<String> c = Comparator.reverseOrder();
              5: var index = Collections.binarySearch(names, "Hoppy", c);
              6: System.out.println(index);
              ```
              The answer happens to be `-­1`. But, you do need to know that the answer 
              is not defined. Line 3 creates a list, `[Fluffy, Hoppy]`. This list happens to be
              sorted in ascending order. Line 4 creates a `Comparator` that reverses the 
              natural order. Line 5 requests a binary search in descending order. _Since 
              the list is not in that order, we don’t meet the precondition for doing a search_.
              The result of calling `binarySearch()` on an improperly sorted list is undefined.
- Collections that require classes to implement
`Comparable`
`TreeMap` and `TreeSet`
  -  Unlike sorting, they don’t check that you have
     implemented `Comparable` at compile time.
      - ```
        2: public class UseTreeSet {
        3:   record Rabbit(int id) {}
        4:   public static void main(String[] args) {
        5:     Set<Duck> ducks = new TreeSet<>();
        6:     ducks.add(new Duck("Puddles"));
        7:
        8:     Set<Rabbit> rabbits = new TreeSet<>();
        9:     rabbits.add(new Rabbit(1)); // ClassCastException
        10: } }
        ```
        - Line 6 is fine. `Duck` does implement `Comparable`. `TreeSet` is able to sort it
          into the proper position in the set. Line 9 is a problem. When `TreeSet`
          tries to sort it, Java discovers the fact that `Rabbit` does not implement
          `Comparable`. Java throws an exception that looks like this:
          ```
          Exception in thread "main" java.lang.ClassCastException:
            class UseTreeSet$Rabbit cannot be cast to class
          java.lang.Comparable
          ```
          - Just like searching and sorting, you can tell collections that require sorting that
           you want to use a specific `Comparator`. For example:
            ```
            8: Set<Rabbit> rabbits = new TreeSet<>((r1, r2) -> r1.id - r2.id);
            9: rabbits.add(new Rabbit(1));
            ```
            - Now Java knows that you want to sort by `id`, and all is well. A
            `Comparator` is a helpful object. It lets you separate sort order from the
            object to be sorted. Notice that line 9 in both of the previous examples
            is the same. It’s the declaration of the `TreeSet` that has changed.
              - **Sorting a List**
                ```
                List<String> bunnies = Arrays.asList("long ear","floppy","hoppy");
                System.out.println(bunnies);    // [long ear, floppy, hoppy]
                bunnies.sort((b1, b2) -> b1.compareTo(b2));
                System.out.println(bunnies);    // [floppy, hoppy, long ear]
                ```
                - There is **not a sort method** on **`Set`** or **`Map`**. 
                Both of those types are unordered, so it
                 wouldn’t make sense to sort them.
- **Summary**
  - ```
    Collections.sort(Collection<T>)
    ```
    - ```
      Collections.sort(Collection<T>,Comparator<T>)
      ```
  - ```
    binarySearch(Collection<T>,T obj)
    ```
    - ```
      binarySearch(Collection<T>,T obj,Comparator<T>)
      ```
  - ```
    List.sort() // Does not exists
    ``` 
    - ```
      List.sort(Comparator<T>)
      ```