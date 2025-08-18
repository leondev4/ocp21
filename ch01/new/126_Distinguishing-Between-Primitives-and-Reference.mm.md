---

markmap:
  initialExpandLevel: 1
---
# **Distinguishing Between Primitives and Reference Types**
- Notice that all the primitive types have lowercase type names. 
All classes that come with Java begin with uppercase.
  - Reference types can be used to call methods, assuming 
  the reference is not `null`. Primitives do not have
   methods declared on them.
- ```java
      4: String reference = "hello";
      5: int len = reference.length();
      6: int bad = len.length(); // DOES NOT COMPILE
  ```
  - We can call a method on `reference` since it is of a reference
  type. You can tell `length` is a method because it has `()` after it.
  - No methods exist on `len` because it is an `int` primitive (line 6). 
  Primitives do not have methods. Remember, a `String` is not a 
  primitive, so you can call methods like `length()` on a `String`
reference, as we did on line 5.
- Reference types can be assigned `null`, which means they do
not currently refer to an object. Primitive types will give you a
compiler error if you attempt to assign them `null`.
  - ```java
    int value = null; //DOES NOT COMPILE
    String name = null;
    ```