---

markmap:
  initialExpandLevel: 1
---
# **Implementing Built-­in<br/> Functional Interfaces**
- `Supplier`
  - A `Supplier` is used when you want to generate or supply
  values without taking any input. The `Supplier` interface
  is defined as follows:
    ```java
    public interface Supplier<T> {
      T get();
    }
    ```
    A `Supplier` is often used when constructing new objects.
    - ```java
      Supplier<LocalDate> s1 = LocalDate::now;
      Supplier<LocalDate> s2 = () -> LocalDate.now();
      LocalDate d1 = s1.get();
      LocalDate d2 = s2.get();
      System.out.println(d1);// 2022-­02-­20
      System.out.println(d2);// 2022-­02-­20
      ```

  - We can print two empty `StringBuilder` objects:
    ```java
    Supplier<StringBuilder> s1 = StringBuilder::new;
    Supplier<StringBuilder> s2 = ()-> new StringBuilder();
    System.out.println(s1.get()); // Empty string
    System.out.println(s2.get()); // Empty string
    ```
    This time, we used a constructor reference to create the object.

    - We’ve been using generics to declare what type of `Supplier` we are using. 
    This can be a little long to read. Can you figure out what the following 
    does? Just take it one step at a time:
      ```java
      Supplier<ArrayList<String>> s3 = ArrayList::new;
      ArrayList<String> a1 = s3.get();
      System.out.println(a1); // []
      ```
      We have a `Supplier` of a certain type. That type happens to be 
      `ArrayList<String>`. Then calling `get()` creates a new instance of 
      `ArrayList<String>`, which is the generic type of the `Supplier`.

      - What would happen if we tried to print out `s3` itself?
      `System.out.println(s3);`
        The code prints something like this:
      `functint.BuiltIns$$Lambda$1/0x0000000800066840@4909b8da`
          That’s the result of calling `toString()` on a lambda. This actually does mean something. 
          Our test class is named `BuiltIns`, and it is in a package that we created named `functint`. 
          Then comes `$$`, which means that the class doesn’t exist in a class file on the file 
          system. It exists only in memory. You don’t need to worry about the rest.
- `Consumer` and <br/>`BiConsumer`
  - You use a `Consumer` when you want to do something 
  with a parameter but not return anything. 
  `BiConsumer` does the same thing, except that it takes 
  two parameters (They don’t have to be the same type.).
    - ```java
      public interface Consumer<T> {
        void accept(T t);
        //omitted default method
      }
      ```
      - ```java
        Consumer<String> c1 = System.out::println;
        Consumer<String> c2 = x->System.out.println(x);

        c1.accept("Annie"); // Annie
        c2.accept("Annie"); // Annie
        ```
    - ```java
      public interface BiConsumer<T, U> {
        void accept(T t, U u);
        //omitted default method
      }
      ```
      - ```java
        var map = new HashMap<String, Integer>();
        BiConsumer<String, Integer> b1 = map::put;
        BiConsumer<String, Integer> b2 = (k, v)-­>map.put(k, v);

        b1.accept("chicken", 7);
        b2.accept("chick", 1);

        System.out.println(map); // {chicken=7, chick=1}
        ```
        - When declaring `b1`, we used an instance 
        method reference on an object since we want
        to call a method on the local variable `map`.
      - ```java
        var map = new HashMap<String, String>();
        BiConsumer<String, String> b1 = map::put;
        BiConsumer<String, String> b2 = (k, v)-­>map.put(k, v);

        b1.accept("chicken", "Cluck");
        b2.accept("chick", "Tweep");

        System.out.println(map); // {chicken=Cluck, chick=Tweep}
        ```
        - This shows that a `BiConsumer` can use the same 
        type for both the `T` and `U` generic parameters.
- `Predicate` and <br/> `BiPredicate`
  - `Predicate` is often used when filtering or matching. 
  A `BiPredicate` is just like a `Predicate`, except 
  that it takes two parameters instead of one. 
    - ```java
      public interface Predicate<T> {
        boolean test(T t);
        //omitted default and static methods
      }
      ```
      - ```java
        Predicate<String> p1 = String::isEmpty;
        Predicate<String> p2 = x -­> x.isEmpty();

        System.out.println(p1.test(""));// true
        System.out.println(p2.test(""));// true
        ```
    - ```java
      public interface BiPredicate<T,U> {
        boolean test(T t, U u);
        //omitted default methods
      }
      ```
      - ```java
        BiPredicate<String, String> b1 = String::startsWith;
        BiPredicate<String, String> b2 = (string, prefix) -­> 
                                  string.startsWith(prefix);
        System.out.println(b1.test("chicken","chick"));// true
        System.out.println(b2.test("chicken","chick"));// true
        ```
        - The method reference includes both the instance variable and
         parameter for `startsWith()`. This is a good example of how
        method references save quite a lot of typing. 
- `Function` and <br/> `BiFunction`
  - A `Function` is responsible for turning one parameter (`T`) 
  into a value of a potentially different type (`R`) and 
  returning it. A `BiFunction` is responsible for turning two 
  parameters (`T`,`U`) into a value (`R`) and returning it.
    - ```java
      public interface Function<T, R> {
        R apply(T t);
        //omitted default and static methods
      }
      ```
      - This function converts a `String` to the length of the `String`:
        ```java
        Function<String, Integer> f1 = String::length;
        Function<String, Integer> f2 = x -> x.length();

        System.out.println(f1.apply("cluck"));// 5
        System.out.println(f2.apply("cluck"));// 5
        ```
        - Technically, it turns the `String` into an `int`, which is autoboxed into an `Integer`.
    - ```java
      public interface BiFunction<T, U, R> {
        R apply(T t, U u);
        //omitted default method
      }
      ```
      - The types don’t have to be different. The following combines two `String` 
      objects and produces another `String`:
        ```java
        BiFunction<String, String, String> b1 = String::concat;
        BiFunction<String, String, String> b2 =
                      (string, toAdd) -> string.concat(toAdd);

        System.out.println(b1.apply("baby ", "chick"));// baby chick
        System.out.println(b2.apply("baby ", "chick"));// baby chick
        ```
        - The first two types in the `BiFunction` are the input types. 
        The third is the result type. For the method reference, 
        the first parameter is the instance that `concat()` is called
        on, and the second is passed to `concat()`.
- `UnaryOperator` and<br/> `BinaryOperator`
  - They require all type parameters to be the same type. 
  A `UnaryOperator` transforms its value into one of the 
  same type. `UnaryOperator` extends `Function`. A 
  `BinaryOperator` merges two values into one of the same
   type. `BinaryOperator` extends `BiFunction`.
    - ```java
      public interface UnaryOperator<T> extends Function<T,T> {
          //omitted static method
      }
      ```
      - ```java
        T apply(T t);
        ```
      - ```java
        UnaryOperator<String> u1 = String::toUpperCase;
        UnaryOperator<String> u2 = x -> x.toUpperCase();

        System.out.println(u1.apply("chirp"));// CHIRP
        System.out.println(u2.apply("chirp"));// CHIRP
        ```
        - We don’t need to specify the return type in the 
        generics because `UnaryOperator` requires it to 
        be the same as the parameter. 
    - ```java
      public interface BinaryOperator<T> extends BiFunction<T, T, T> {
          // omitted static methods
      }
      ```
      - ```java
        T apply(T t1, T t2);
        ```
      - ```java
        BinaryOperator<String> b1 = String::concat;
        BinaryOperator<String> b2 = (string, toAdd) -> string.concat(toAdd);

        System.out.println(b1.apply("baby ", "chick"));// baby chick
        System.out.println(b2.apply("baby ", "chick"));// baby chick
        ```