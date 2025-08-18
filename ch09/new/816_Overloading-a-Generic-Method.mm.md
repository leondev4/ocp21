---

markmap:
  initialExpandLevel: 1
---
# **Overloading and overriding <br/>a Generic Method**

- In a parameter like **`Type<T>`**, you _must change 
`Type` and optional `T`_, only changing `T` is not 
considered overloading.
  - ```
    protected void chew(List<Object> input) {}
    protected void chew(List<Double> input) {}// DOES NOT COMPILE
    ```
  - Also apply to parent class and child:
    ```java
    public class LongTailAnimal {
      protected void chew(List<Object> input) {}
    }
    public class Anteater extends LongTailAnimal {
      protected void chew(List<Double> input) {} // DOES NOT COMPILE
    }
    ```
  - Ok:
    ```java
    public class LongTailAnimal {
      void chew(List<Object> input) {}
    }
    public class Anteater extends LongTailAnimal {
      protected void chew(List<Object> input) {} //overriding
      protected void chew(ArrayList<Double> input) {}//overloading
    }
    ```
- To override a method: parameter ****`Type<T>`** must same 
in both parent and child**, see example. In return's type 
must be covariant in `Type` and same in `T`, but you can 
also use wildcards in `Type`.
  - ```
    public class LongTailAnimal {
      List<String> chew(List<Object> input) {return null;}
      List<? extends Number> chew(){return null;}
      List<? super Integer> chew(int x){return null;}
    }
    public class Anteater extends LongTailAnimal {
      protected ArrayList<String> chew(List<Object> input) {return null;}
      List<Integer> chew() {return null;}
      ArrayList<Number> chew(int x){return null;}
    }
    ```
  - Remember:
    **`<? extends Number>`**: inside **`<>`** you can pass `Number` or its subtypes (`Integer`, `Double`, `Float`,..), 
    also known as upper bounds. You cannot add, but you can remove.
    <br/>

    **`<? super Integer>`**: inside **`<>`** you can pass `Integer` or its parents (`Number` or `Object`), also kown 
    as lower bounds and `Type` is modifiable for example `List<? super Number> modifiable` you can add 
    `Number` or its childs (`Integer`, `Float`, `Double`). 
    ```
    modifiable.add(1);  
    modifiable.add(1.0); 
    modifiable.add(3.1F);
    ```
    <br/>

    **`<?>`** inside **`<>`** you can pass anything also known unbounded you can see **`List<?>`**  like **`List<Object>`**