---

markmap:
  initialExpandLevel: 1
---
# **Writing Literals**
- When a number is present in the code, it is called a _literal_. By
default, Java assumes you are defining an `int` value with a
numeric literal. In the following example, the number listed is
bigger than what fits in an `int`.
  -   ```
      long max = 3123456789; //DOES NOT COMPILE
      ```
      - Java complains the number is out of range. And it is—for an `int`.
      - The solution is to add the character `L` to the number.
        ```java
        long max = 3123456789L; // Now Java knows it is a long
        ```
        Alternatively, you could add a lowercase `l` to the number.
- Java allows you to specify digits in several other formats.
  - Octal (digits 0–7), which uses the number**0 as a prefix**—for
example, 017.
  - Hexadecimal (digits 0–9 and letters A–F/a–f), which uses**0x or 0X 
  as a prefix**—for example, 0xFF, 0xff, 0XFf. **Hexadecimal is
   case insensitive**, so all of these examples mean the same value.
  - Binary (digits 0–1), which uses the number **0b or 0B as a
   prefix**—for example, 0b10, 0B10.
- **Literals and the Underscore Character**
  - You can’t place an underscore right after the prefixes `0b`, `0B`, 
  `0x`, and `0X` or before `b`,`B`, `x` or `X`.
    - ```
        int x1 = 0b_11;     //DOES NOT COMPILE
        int x2 = 0_x11;     //DOES NOT COMPILE
        int x3 = 0xA_B_CD_F;//compiles
        ```
  - You can’t place an underscore prior to an `L`, `F` or `D` suffix.
    - ```java
      long x1 = 11_L;     //DOES NOT COMPILE
      double x2 = 0.1D_;   //DOES NOT COMPILE
      float x3 = 1.0_4F;   //compiles
      ```
  - You can add underscores anywhere except at the beginning of a
  literal, the end of a literal, right before a decimal point, or right 
  after a decimal point. You can even place multiple underscore
  characters next to each other.
    - ```
      double notAtStart = _1000.00;     // DOES NOT COMPILE
      double notAtEnd = 1000.00_;       // DOES NOT COMPILE
      double notByDecimal = 1000_.00;   // DOES NOT COMPILE
      double annoyingButLegal = 1_00_0.0_0; // Ugly, but compiles
      double reallyUgly = 1____2; // Also compiles
      ```