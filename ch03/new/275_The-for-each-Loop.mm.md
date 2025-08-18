---

markmap:
  initialExpandLevel: 1
---
# **The for-each Loop**
- The for-each loop is a specialized structure designed to
iterate over arrays and various Collections Framework
classes, as presented in [Figure 3.11](https://1drv.ms/i/c/c83cfca51d5c2032/EbBhsnCqspZAjjNkJHm2THYB_An-3U5Iyx7_DZEbbDK5Fg?e=QDfojw).
  - The for-each loop declaration is composed of an initialization 
  section and an object to be iterated over. The right side of the 
  for-each loop must be one of the following:
    - \+ A built-in Java array
      \+ An object whose type implements `java.lang.Iterable`
      - The right side must be an array or collection 
      of items, such as a `List` or a `Set`.
      - `Map` is not supported in a for-each loop, although `Map` 
      does include methods that return `Collection` instances.
        - The left side of the for-each loop must include a declaration
        for an instance of a variable whose type is compatible with
        the type of the array or collection on the right side of the
        statement. On each iteration of the loop, the named
        variable on the left side of the statement is assigned a new
        value from the array or collection on the right side of the
        statement.
- Compare these two methods that both print the values of an array, 
one using a traditional for loop and the other using a for-each loop:
  - ```java
    void printNames(String[] names) {
      for(int counter=0; counter < names.length; counter++)
        System.out.println(names[counter]);
      }

    void printNames(String[] names) {
      for(var name : names)
        System.out.println(name);
    }
    ```
    - The for-each loop is a lot shorter, isn’t it? We no longer
have a `counter` loop variable that we need to create,
increment, and monitor.
      - We can also use a for-each loop on a `List`, since it
implements `Iterable`.
      - ```java
        void printNames(List<String> names) {
          for (var name : names)
            System.out.println(name);
        }
        ```
        - You just need to know that on each iteration, a for-each loop assigns
         a variable with the same type as the generic argument. In this case, 
         `name` is of type `String`.
- What about the following examples?
  ```java
  String birds = "Jay";
  for(String bird :birds) //DOES NOT COMPILE
    System.out.print(bird + " ");

  String[] sloths = new String[3];
  for (int sloth: sloths) //DOES NOT COMPILE
    System.out.print(sloth + " ");
  ```
  - The first for-each loop does not compile because the `String
birds` 
cannot be used on the right side of the statement.
  - The second example does not compile because the loop variable type 
  on the left side of the statement is `int` and doesn’t match the expected 
  type of `String`.