---

markmap:
  initialExpandLevel: 1
---
# **Using Reference Types**
- A reference type refers to an object. Primitive types hold their values in
the memory where the variable is allocated, references hold the memory
address where the object is located, a concept referred to as a pointer
- Suppose we declare a reference of type `String`.
- `String greeting;`
  - The `greeting` variable is a reference that can only point to a 
  `String` object. A value is assigned to a reference in one of two ways.
  - 1. A reference can be assigned to another object of the same or
compatible type.
        - ```
          String hello= new String("hello");
          greeting = hello;
          ```
  - 2. A reference can be assigned to a new object using the `new`
keyword.
        - ```java
          greeting = new String("How are you?");
          ```
        - The `greeting` reference points to a new `String` object, `"How are you?"`.
        **The `String` object does not have a name and can be accessed only
        via a corresponding reference.**
- **Objects vs. References**
  - The reference is a variable that has a name and can be used to access 
  the contents of an object. A reference can be assigned to another 
  reference, passed to a method, or returned from a method. All 
  references are the same size, no matter what their type is.
  - An object sits on the heap and does not have a name. Therefore, you 
  have no way to access an object except through a reference. Objects 
  come in all different shapes and sizes and consume varying amounts 
  of memory. An object cannot be assigned to another object, and an 
  object cannot be passed to a method or returned from a method. It is 
  the object that gets garbage collected, not its reference.