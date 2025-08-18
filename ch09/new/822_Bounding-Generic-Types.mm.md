---

markmap:
  initialExpandLevel: 1
---
# **Bouding generic class or method**
- You can use `<T extends Type>` when declare a generic classs or method. 
- ```
  class MyClass <T extends Number>{
    static <T extends CharSequense> T myMethod(){
      return null;
    }
  }
  ```
  - In `MyClass`, `T` can be `Number` or its subtypes
  - In `myMethod`, `T` can be `CharSequence` or its subtypes
  - `T` in `MyClass` and `T` in `myMethod` are different types.
- **You cannot use wildcards**.
  - ```
    class MyClass <? extends Number>{
      <? extends CharSequense> ? myMethod(){
          return null;
      }
    }
    ```
    - Compile error in `MyClass` and `myMethod` declarations
- You can use wildcards in return's type method and method parameters, in 
form `TypeVariable<? superORextends ClassOrInterface>`. For example:
- ```
  class MyClass {
    List<? extends CharSequence> myMethod(List<? super Integer> list){
      return null;
    }
  }
  ```
  - You can return:
`List` or `ArrayList`
    - Inside `<>`:
`CharSequence`, `String` or 
`StringBuilder`
  - You can pass when call 
    method:
    `List` or `ArrayList`
    - Formal paramter type:
    inside `<>`: `Integer`, 
    `Number` or  `Object`
- ```
  class MyClass {
    List<? super Integer> myMethod(List<? extends CharSequence> list){
      return null;
    }
  }
  ```
  - You can return:
`List` or `ArrayList`
    - Formal paramter type:
    Inside `<>`:`Integer`, 
    `Number` or `Object`
  - You can pass when call 
method:
`List` or `ArrayList`
    - Inside `<>`:
  `CharSequence`, `String` or 
  `StringBuilder`
- **Bounding Generic Types**
  - Bounded wildcards restricting what
  types can be used in a wildcard
    - A _bounded parameter type_ is a generic type
    that specifies a bound for the generic.
    Only permits `<T extends Type>`
      - A _wildcard generic type_ is an unknown generic 
      type represented with a question mark (`?`)
  - Unbounded wildcard
    - `?`
      - ```java
        List<?> a = new ArrayList<String>();
        ```
        - You can assign to `a`, `ArrayList` or `List` inside `<>` put `<Anything>`
        - You can imagine `Lst<?>` like `List<Object>`
        - You can only add `null` or remove elements
  - Wildcard with upper
  bound
    - `? extends type`
      - ```java
        List<? extends Number> a = new ArrayList<Long>();
        ```
        - You can assign to `a`, `ArrayList` or `List` inside `<>` put `<Number or its childs>`
        - You can only add `null` or remove elements
  - Wildcard with lower
    bound
      - `? super type`
        - ```java
          List<? super Long> a = new ArrayList<Number>();
          ```
          - You can add `Long` or its subtypes and remove
          - You can assign to `a`, `ArrayList` or `List` inside `<>` put `<Long or its parents>`