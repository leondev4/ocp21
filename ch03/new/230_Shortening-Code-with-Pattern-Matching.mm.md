---

markmap:
  initialExpandLevel: 1
---
# **Shortening Code with <br/> Pattern Matching**
- _Pattern matching_ is a technique of controlling program flow that only 
executes a section of code that meets certain criteria. We perform 
pattern matching with the `if` statement, along with the `instanceof` 
operator, to improve program control.
  - Pattern matching is a useful tool for reducing boilerplate
    code in your application. _Boilerplate_ code is code that tends
    to be duplicated throughout a section of code over and over
    again in a similar manner.
    - To understand why this feature was added, consider the following code 
    that takes a `Number` instance and compares it with the value `5`. You just 
    need to know that `Integer` inherits from `Number`.
      - ```java
        void compareIntegers(Number number) {
          if(number instanceof Integer) {
            Integer data =(Integer)number;
            System.out.print(data.compareTo(5));
          }
        }
        ```
        - The cast is needed since the `compareTo()` method
        is defined on `Integer`, but not on `Number`.
- Code that first checks if a variable is of a particular type and then 
immediately casts it to that type is extremely common in the Java 
world. It’s so common that the authors of Java decided to implement
 a shorter syntax for it:
  - ```java
    void compareIntegers(Number number) {
      if(number instanceof Integer data ){
        System.out.print(data.compareTo(5));
      }
    }
    ```
    - The variable `data` in this example is referred to as the
**pattern variable**. Notice that this code also avoids any
potential `ClassCastException` because the cast operation 
is executed only if the `instanceof` operator returns `true`.
      - [Figure 3.3](https://1drv.ms/i/c/c83cfca51d5c2032/EQPN6t4VJH1Jr1D6HVmTn3EB6M-OLAiC1st9dyxllhzL8g?e=Ooj5uL) shows the anatomy of pattern matching using the `instanceof` 
      operator and `if` statements. Adding a variable after the type is what 
      instructs the compiler to treat it as pattern matching. The Figure also 
      shows an optional conditional clause.
        - **Reassigning Pattern Variables**
          While possible, it is a bad practice.
          
          ```java
          if(number instanceof Integer data) {
            data = 10;
          }
          ```
          The reassignment can be prevented with a `final`
          modifier.
          ```java
          if(number instanceof final Integer data){
            data = 10; // DOES NOT COMPILE
          }
          ```
- **Pattern Variables and Expressions**
Pattern matching supports an optional `boolean` 
expression. This can be used to filter data out.
  - ```java
    void printIntegersGreaterThan5(Number number){
      if(number instanceof Integer data && data.compareTo(5) > 0)
        System.out.print(data);
    }
    ```
    - We can apply a number of filters, or patterns, so that the `if` statement  is executed 
    only in specific circumstances. **Notice that we’re using  the pattern variable 
    in an expression in the same line in which it is declared.**
      - **Pattern Matching with `null`**
      The `instanceof` operator always evaluates `null` references 
      to `false`. The same holds for pattern matching.
        - ```java
          String noObjectHere = null;

          if(noObjectHere instanceof String)
              System.out.println("Not printed");

          if(noObjectHere instanceof String s)
              System.out.println("Still not printed");

          if(noObjectHere instanceof String s && s.length() > -1)
            System.out.println("Nope, not this one either");
            ```
      - As shown in the last example, this also helps avoid any potential `NullPointerException`, 
      as the conditional operator (`&&`, short-circuit) causes the `s.length()` call to be skipped.
- **Supported Types**
The type of the pattern variable must be a compatible type,
which includes the **same type, a subtype, or a supertype 
of the reference variable. If the reference variable does 
not refer to a `final` class or type, then it can also
include an unrelated interface.**
  - Consider the following two examples, in which 
  `Integer` is a subtype of `Number`:
    - ```java
      11: Number bearHeight = Integer.valueOf(123);
      12:
      13: if (bearHeight instanceof Integer i){}
      14: if (bearHeight instanceof Number n){}
      15: if (bearHeight instanceof String s){} // DOES NOT COMPILE
      16: if (bearHeight instanceof Object o){}
      17: if (bearHeight instanceof Cloneable c) {}
      ```
      - The first example uses a subtype, while the second example uses the same type as the reference variable
      `bearHeight`. On line `15`, the compiler recognizes that a `Number` cannot be cast to an unrelated type
      `String` and throws an error. Line 16 is permitted but not particularly useful, since every `Object` except
      `null` will return `true`.
      Applies to the line 17: If the reference variable does not refer to a `final` class or type, it can also include an 
      unrelated interface.
- **Flow Scoping**
Flow scoping means the variable is only in scope when the
compiler can definitively determine its type. Flow scoping 
is unlike any other type of scoping, in that it is not strictly 
hierarchical. It is determined by the compiler based on the
 branching and flow of the program.
  - ```java
    void printIntegersOrNumbersGreaterThan5(Number number) {
      if(number instanceof Integer data || data.compareTo(5) > 0)
        System.out.print(data);
    }
    ```
    - The key thing to notice is that we used OR (`||`) not AND (`&&`) in the 
    conditional statement. If the input does not inherit `Integer`, the `data` 
    variable is undefined. Since the compiler cannot guarantee that 
    `data` is an instance of `Integer`, the code does not compile.
      - ```java
        void printIntegerTwice(Number number) {
          if(number instanceof Integer data)
            System.out.print(data.intValue());
            var n =data.intValue(); // DOES NOT COMPILE
        }
        ```
        - Since the input might not have inherited `Integer`, `data` is no
longer in scope after the `if` statement. 
- Consider the following example that does compile:
  ```java
  void printOnlyIntegers(Number number) {
    if(!(number instanceof Integer data))
      return;
    var n =data.intValue();
  }
  ```
  - It might surprise you to learn this code does compile. The 
  method returns if the input does not inherit `Integer`. This 
  means that when the last line of the method is reached, 
  the input must inherit `Integer`, and therefore `data` 
  stays in scope even after the `if` statement ends.
    - **Flow Scoping and `else` Branches**
      - Another way to think about it is to rewrite the
      logic to something equivalent that uses an `else`
      statement:
        - ```java
          void printOnlyIntegers(Number number) {
            if(!(number instanceof Integer data))
              return;
            else
              System.out.print(data.intValue());
          }
          ```
      - We can now go one step further and reverse the `if` and
`else` branches by inverting the `boolean` expression:
        - ```java
          void printOnlyIntegers(Number number) {
            if(number instanceof Integer data)
              System.out.print(data.intValue());
            else
              return;
          }
          ```
          - Our new code is equivalent to our original and better demonstrates 
          how the compiler was able to determine that `data` was in scope 
          only when `number` is an `Integer`.