---

markmap:
  initialExpandLevel: 1
---
# **Casting Values**
- _Casting_ is a unary operation where one data type is explicitly 
interpreted as another data type. Casting is optional and
unnecessary when converting to a larger or widening data
type, but it is required when converting to a smaller or
narrowing data type. Without casting, the compiler will
generate an error when trying to put a larger data type inside 
a smaller one.
  - **Casting is performed by placing the data type, enclosed in 
  parentheses, to the left of the value you want to cast.**
    - ```java
      int fur =(int)5;
      int hair =(short)2;
      String type =(String)"Bird";
      short tail =(short)(4 + 10);
      long feathers = 10(long); // DOES NOT COMPILE
      ```
        - Spaces between the cast and the value are optional. It is common for the right 
        side to also be in parentheses. Since casting is a unary operation, it would only 
        be applied to the `4` if we didn’t enclose `4 + 10` in parentheses. The last example 
        does not compile because the type is on the wrong side of the value.
    - The compiler automatically casts smaller data types to larger ones.
- See if you can figure out why none of the 
following lines of code compiles:
  - ```java
    float egg = 2.0 / 9;       // DOES NOT COMPILE
    int tadpole = (int)5 * 2L; // DOES NOT COMPILE
    short frog = 3 - 2.0;      // DOES NOT COMPILE
    int tadpole = 5 *(int) 2L; // IT COMPILES
    ```
    - The first three examples involve putting a larger value into a
    smaller data type.
- See if you can figure out why each of the 
following lines does not compile:
  - ```java
    int fish = 1.0;                         // DOES NOT COMPILE
    short bird = 1921222;                   // DOES NOT COMPILE
    int mammal = 9f;                        // DOES NOT COMPILE
    long reptile = 192_301_398_193_810_323; // DOES NOT COMPILE
    ```
    - The first statement does not compile because you are trying to assign a `double` `1.0` to an integer value.
    Even though the value is a mathematic integer, by adding `.0`, you’re instructing the compiler to treat it 
    as a `double`. The second statement does not compile because the literal value `1921222` is outside the 
    range of `short`, and the compiler detects this. The third statement does not compile because the `f` 
    added to the end of the number instructs the compiler to treat the number as a floating-point value, but 
    the assignment is to an `int`. Finally, the last statement does not compile because Java interprets the
    literal as an `int` and notices that the value is larger than `int` allows. The literal would need a postfix `L` 
    or `l` to be considered a `long`.
- Casting primitives is required any time you are going from a 
larger numerical data type to a smaller numerical data type,
or converting from a floating-point number to an integral value.
  - ```java
    int fish = (int)1.0;
    short bird = (short)1921222; // Stored as 20678
    int mammal = (int)9f;
    ```
    - What about applying casting to an earlier example?
      ```java
      long reptile = (long)192301398193810323; // DOES NOT COMPILE
      ```
      - This still does not compile because the value is first interpreted 
      as an `int` by the compiler and is out of range. The following fixes 
      this code without requiring casting:
        ```java
        long reptile = 192301398193810323L;
        ```
- Let’s return to a similar example from the “Numeric
Promotion”
  - ```java
    short mouse = 10;
    short hamster = 3;
    short capybara = mouse * hamster; // DOES NOT COMPILE
    ```
    - As you may remember, `short` values are automatically promoted to 
    `int` when applying any arithmetic operator, with the resulting value 
    being of type `int`. Trying to assign a `short` variable with an `int` value 
    results in a compiler error, as Java thinks you are trying to implicitly 
    convert from a larger data type to a smaller one.
      - In this example, we know the result of `10 * 3` is `30`,
      which can easily fit into a short variable, so we can apply
      casting to convert the result back to a `short`:
      -  ```java
          short mouse = 10;
          short hamster = 3;
          short capybara = (short)(mouse * hamster);
          ```
          By casting a larger value into a smaller data type, you
          instruct the compiler to ignore its default behavior.
- Casting can appear anywhere in an
expression, not just on the assignment.
  - ```java
    short mouse = 10;
    short hamster = 3;
    short capybara = (short)mouse * hamster;// DOES NOT COMPILE
    ```
    - The cast in the last line is applied to `mouse`, and `mouse` alone. After the cast 
    is complete, both operands are promoted to `int` since they are used with the 
    binary multiplication operator (`*`), making the result an `int` and causing a 
    compiler error.
      - What if we changed the last line to the following?
        ```java
        short capybara = 1+(short)(mouse*hamster); // DOES NOT COMPILE
        ```
        In the example, casting is performed successfully, but the resulting value is 
        automatically promoted to `int` because it is used with the binary arithmetic 
        operator (`+`).
- **Casting Values vs. Variables**
The compiler doesn’t require casting when working 
with literal values that fit into the data type. 
  - ```java
    byte hat = 1;
    byte gloves = 7 * 10;
    short scarf = 5;
    short boots = 2 + 1;
    ```
    - All of these statements compile without issue. 
    On the other hand, neither of these statements 
    compiles:
      - ```java
        short boots = 2 + hat; // DOES NOT COMPILE
        byte gloves = 7 * 100; // DOES NOT COMPILE
        short boots2 = boots; // COMPILES
        ```
        - The first statement does not compile because `hat` is a variable, 
        not a value, and both operands are automatically promoted to `int`. 
        - The second expression does not compile because `700` triggers 
        an overflow for `byte`, which has a maximum value of `127`.