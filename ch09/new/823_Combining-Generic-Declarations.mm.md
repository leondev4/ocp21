---

markmap:
  initialExpandLevel: 2
---
# **Combining Generic Declarations**
- We have:
  ```
  class A {}
  class B extends A {}
  class C extends B {}
  ```
- Can you figure out why these do or don’t compile? Also, try to
  figure out what they do.
  ```js
  6: List<?> list1 = new ArrayList<A>();
  7: List<? extends A> list2 = new ArrayList<A>();
  8: List<? super A> list3 = new ArrayList<A>();
  ```
  - Line 6 creates an `ArrayList` that can hold instances of class `A`. It is
  stored in a variable with an unbounded wildcard. Any generic type can
  be referenced from an unbounded wildcard, making this OK.
  - Line 7 tries to store a list in a variable declaration with an upper-
  bounded wildcard. This is OK. You can have `ArrayList<A>`, `ArrayList<B>`,
  or `ArrayList<C>` stored in that reference. Line 8 is also OK. This time,
  you have a lower-bounded wildcard. The lowest type you can
  reference is A. Since that is what you have, it compiles.
- Did you get those right? Let’s try a few more.
  ```js
   9: List<? extends B> list4 = new ArrayList<A>(); // DOES NOT COMPILE
  10: List<? super B> list5 = new ArrayList<A>();
  11: List<?> list6 = new ArrayList<? extends A>(); // DOES NOT COMPILE
  ```
  - Line 9 has an upper-bounded wildcard that allows `ArrayList<B>` or
  `ArrayList<C>` to be referenced. Since you have `ArrayList<A>` that is trying
  to be referenced, the code does not compile. Line 10 has a lower-
  bounded wildcard, which allows a reference to `ArrayList<A>`,
  `ArrayList<A>`, or `ArrayList<Object>`.
  - Finally, line 11 allows a reference to any generic type since it is an
unbounded wildcard. The problem is that you need to know what that
type will be when instantiating the `ArrayList`. It wouldn’t be useful
anyway, because you can’t add any elements to that `ArrayList`.
# **Passing Generic Arguments**
- ```
  <T> T first(List<? extends T> list) {
    return list.get(0);
  }
  ```
  - This  method is a perfectly normal use of generics. It uses a 
  method-specific type parameter, `T`. It takes a parameter of `List<T>`, or
  some subclass of `T`, and it returns a single object of that `T` type. For
  example, you could call it with a `List<String>` parameter and have it
  return a `String`. Or you could call it with a `List<Number>` parameter and
  have it return a `Number`.
- ```
  <T> <? extends T> second(List<? extends T> list) { // DOES NOT COMPILE
    return list.get(0);
  }// Wildcard alone is not a return type
  ```
  - This method does not compile because the return type isn’t actually 
  a type. You are writing the method. You know what type it is 
  supposed to return. You don’t get to specify this as a wildcard.
- ```
  <B extends A> B third(List<B> list) {
    return new B(); // DOES NOT COMPILE
  }
  ```
  - This method, does not compile. `<B extends A>` says that you want 
  to use `B` as a type parameter just for this method and that it needs 
  to extend the `A` class. Coincidentally, `B` is also the name of a class. 
  Well, it isn’t a coincidence. It’s an evil trick. Within the scope of the 
  method, `B` can represent class `A`, `B`, or `C`, because all extend the 
  `A` class. Since `B` no longer refers to the `B` class in the method, you 
  can’t instantiate it.
- ```
  void fourth(List<? super B> list) {}
  ```
  - This is a normal use of generics. You can pass the type `List<B>`, `List<A>`,
   or `List<Object>`. You can change `List` to `ArrayList` and it compiles. 
- ```
  <X> void fifth(List<X super B> list) { } // DOES NOT COMPILE
  ```
  - This method does not compile because it tries to mix 
  a method-specific type parameter with a wildcard. A 
  wildcard must have a `?` in it.