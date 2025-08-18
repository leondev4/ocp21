---

markmap:
  initialExpandLevel: 1
---
# **Creating Wrapper Classes**
- Each primitive type has a wrapper class, which is an object type
that corresponds to the primitive.
- **Primitive
  type**
  - **Wrapper
  class**
    - **Wrapper class
    inherits `Number`?**
      - **Example of creating**
        - **Common methods**
- `boolean`
  - `Boolean`
    - NO
      - &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; `Boolean.valueOf(true);`
        - `public boolean booleanValue()`
        - `public static boolean parseBoolean(String s)`
        - `public static Boolean valueOf(String s)`
        - `public static Boolean valueOf(boolean b)`
- `char`
  - `Character`
    - NO
      - &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; `Character.valueOf('c')`
        - `public char charValue()`
        - `public static Character valueOf(char c)`
- `byte`
  - `Byte`
    - YES
      - `Byte.valueOf((byte) 1)`
- `short`
  - `Short`
    - YES
      - `Short.valueOf((short)1)`
- `int`
  - `Integer`
    - YES
      - `Integer.valueOf(1)`
        - Byte, Short, Integer or Long
          - `public xxx xxxValue()`
          - `public static xxx parseXxx(String s)`
          - `public static XXX valueOf(String s)`
          - `public static XXX valueOf(xxx v)`
          - `public static XXX valueOf(String s, int radix)`
- `long`
  - `Long`
    - YES
      - `Long.valueOf(1)`
        `Long.valueOf(1L)`
- `float`
  - `Float`
    - YES
      - `Float.valueOf((float)1.0)`
        `Float.valueOf(1.0F)`
          - `Double` or `Float`
            - `public static XXX valueOf(String s)`
            - `public static XXX valueOf(xxx n)`
- `double`
  - `Double`
    - YES
      - `Double.valueOf(1.0)`
- xxx or Xxx
  - XXX