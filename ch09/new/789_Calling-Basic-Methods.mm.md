---

markmap:
  initialExpandLevel: 1
---
# **Methods of `Map`**
- **Calling Basic Methods**
  - ```java
    void addElementsAndPrint(Map<String, String> map) {
      map.put("koala", "bamboo");
      map.put("lion", "meat");
      map.put("giraffe", "leaf");
      String food = map.get("koala"); // bamboo
      for (String key: map.keySet())
        System.out.print(key + ",");
    }
    ```
    - ```java
      addElementsAndPrint(new HashMap<>());       // koala,giraffe,lion,
      addElementsAndPrint(new LinkedHashMap<>()); // koala,lion,giraffe,
      addElementsAndPrint(new TreeMap<>());       // giraffe,koala,lion,
      ```
      - Like `Set` classes, `HashMap` prints the elements in an arbitrary ordering 
      using the `hashCode()` of the key. `LinkedHashMap` prints the elements 
      in the order in which they were inserted. Finally, `TreeMap` prints the 
      elements based on the order of the keys.
        - Using our `HashMap` instance, we can try some boolean checks.
          ```java
          System.out.println(map.containsKey("lion"));  //true
          System.out.println(map.containsValue("lion"));//false
          System.out.println(map.size());               // 3
          map.clear();                                  // {}
          System.out.println(map.size());               // 0
          System.out.println(map.isEmpty());            // true
          ```
          - Do you see why this doesn’t compile?
            ```java
            System.out.println(map.contains("lion")); // DOES NOT COMPILE
            ```
            It doesn’t compile because the `contains()` method is on the `Collection`
            interface but not the `Map` interface
- **Iterating through a `Map`**
  - ```java
    Map<Integer, Character> map = new HashMap<>();
    map.put(1, 'a'); map.put(2, 'b'); map.put(3, 'c');
    map.forEach((k, v) -> System.out.println(v));
    ```
    - Since we don’t care about the key, this particular code could have been 
    written with the `values()` method and a method reference instead.
      ```java
      map.values().forEach(System.out::println);
      ```
      - Another way of iterating over the data in a map is using `entrySet()`,
      which returns a set of `Map.Entry<K, V>` objects.
        ```java
        map.entrySet().forEach(e ->
          System.out.println(e.getKey() + " " + e.getValue()));
        ```
        In this case, each element `e` is of type `Map.Entry<Integer, Character>`.
- **Getting Values Safely**
  - ```java
    3: Map<Character, String> map = new HashMap<>();
    4: map.put('x', "spot");
    5: System.out.println("X marks the " + map.get('x'));
    6: System.out.println("X marks the " + map.getOrDefault('x', "@"));
    7: System.out.println("Y marks the " + map.get('y'));
    8: System.out.println("Y marks the " + map.getOrDefault('y', "@"));
    ```
    - This code prints the following:
      ```java
      X marks the spot
      X marks the spot
      Y marks the null
      Y marks the @
      ```
      - **Replacing Values**
        ```java
        21: Map<Integer, Integer> map = new HashMap<>();
        22: map.put(1, 2);
        23: map.put(2, 4);
        24: Integer original = map.replace(2, 10); // 4
        25: System.out.println(map);               // {1=2, 2=10}
        26: map.replaceAll((k, v) -> k + v);
        27: System.out.println(map);               // {1=3, 2=12}
        ```
        - Note that `replace()` and `replaceAll()` do not modify the `Map` if it does
        not contain the key. Contrast this with `put()`, which will always
        attempt to set a value.
          ```java
          Map<Integer, Integer> map = new HashMap<>();
          map.put(1, 2); //{1=2}
          map.put(2, 4); //{1=2, 2=4}
          map.put(2,20); //{1=2, 2=20}
          ```
          - **Putting If Absent**
            The `putIfAbsent()` method sets a value in the map but skips it if the value is already 
            set to a non-`null` value.
            ```java
            Map<String, String> favorites = new HashMap<>();
            favorites.put("Jenny", "Bus Tour");
            favorites.put("Tom", null);
            favorites.putIfAbsent("Jenny", "Tram");
            favorites.putIfAbsent("Sam", "Tram");
            favorites.putIfAbsent("Tom", "Tram");
            System.out.println(favorites); // {Tom=Tram, Jenny=Bus Tour,Sam=Tram}
            ```
            - As you can see, `Jenny`’s value is not updated because one was already
present. `Sam` wasn’t there at all, so he was added. `Tom` was present as a
key but had a `null` value. Therefore, he was updated as well.
- **Merging Data**
The `merge()` method adds logic of what to choose. Suppose we want to
choose the ride with the longest name. We can write code to express
this by passing a mapping function to the `merge()` method.
  - ```java
    11: BiFunction<String, String, String> mapper = (v1, v2)
    12:       -> v1.length()> v2.length() ? v1: v2;
    13:
    14: Map<String, String> favorites = new HashMap<>();
    15: favorites.put("Jenny", "Bus Tour");
    16: favorites.put("Tom", "Tram");
    17:
    18: String jenny = favorites.merge("Jenny", "Skyride", mapper);
    19: String tom = favorites.merge("Tom", "Skyride", mapper);
    20:
    21: System.out.println(favorites); // {Tom=Skyride, Jenny=Bus Tour}
    22: System.out.println(jenny);     // Bus Tour
    23: System.out.println(tom);       // Skyride
    ```
    - The code on lines 11 and 12 takes two parameters and returns a
      value. Our implementation returns the one with the longest name.
      Line 18 calls this mapping function, and it sees that `Bus Tour` is longer
      than `Skyride`, so it leaves the value as `Bus Tour`. Line 19 calls this
      mapping function again. This time, `Tram` is shorter than `Skyride`, so the
      map is updated. Line 21 prints out the new map contents. Lines 22
      and 23 show that the result is returned from `merge()`.
      -  The `merge()` method also has logic for what happens if `null` values or
      missing keys are involved. In this case, it doesn’t call the `BiFunction` at
      all, and it simply uses the new value.
          ```java
          BiFunction<String, String, String> mapper =
            (v1, v2) -> v1.length()> v2.length() ? v1 : v2;
          Map<String, String> favorites = new HashMap<>();
          favorites.put("Sam", null);
          favorites.merge("Tom", "Skyride", mapper);
          favorites.merge("Sam", "Skyride", mapper);
          System.out.println(favorites); // {Tom=Skyride, Sam=Skyride}
          ```
          - Notice that the mapping function isn’t called. If it were, we’d have a
          `NullPointerException`. The mapping function is used only when there
          are two actual values to decide between.
            - The final thing to know about `merge()` is what happens when the
            mapping function is called and returns `null`. The key is removed from
            the map when this happens.
              ```java
              BiFunction<String, String, String> mapper = (v1, v2) -> null;
              Map<String, String> favorites = new HashMap<>();
              favorites.put("Jenny", "Bus Tour");
              favorites.put("Tom", "Bus Tour");
              favorites.merge("Jenny", "Skyride", mapper);
              favorites.merge("Sam", "Skyride", mapper);
              System.out.println(favorites); // {Tom=Bus Tour, Sam=Skyride}
              ```
              - Tom was left alone since there was no `merge()` call for that key. Sam
was added since that key was not in the original list. Jenny was
removed because the mapping function returned `null`.
- **Behavior of the `merge()` method**
  - If the requested key
    - Has a `null` value in map
      - mapping function is not called
        - Update key's value in map with value parameter
    - Has a non-`null` value in map
      - mapping function returns `null`
        - Remove key form map
      - mapping function returns a non-`null` value
        - Set value to mapping function result
    - Is not in map
      - mapping function is not called
        - Add key with value parameter to map.