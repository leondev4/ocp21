---

markmap:
  initialExpandLevel: 1
---
# **The Primitive Types**
- Java has eight built-in data types, referred to as the Java _primitive types_.
  - A primitive is not an object in Java, nor does it represent an object. 
  A primitive is just a single value in memory, such as a number or character.
- **Table 1.5** shows the Java primitive types together with their size in bits
and the range of values that each holds.
  - **TABLE 1.5**
    |Keyword| Type| Min value| Max value| Default value| Example|
    |-|-|-|-|-|-|
    |`boolean`|`true` or `false`|n/a|n/a|`false`|`true`|
    |`byte`|8-­bit integral value|$-­128=-2^7$|$127=2^7-1$|`0`|`123`|
    |`short`|16-­bit integral value|$-­32,768=-2^{15}$|$32,767=2^{15}-1$|`0`|`123`|
    |`int`|32-­bit integral value|$-­2,147,483,648=-2^{31}$|$2,147,483,647=2^{31}-1$|`0`|`123`|
    |`long`|64-­bit integral value|$-2^{63}$|$2^{63}-1$|`0L`|`123L`|
    |`float`|32-­bit floating-­point value|n/a|n/a|`0.0f`|`123.45f`|
    |`double`|64-­bit floating-­point value|n/a|n/a|`0.0`|`123.456`|
    |`char`|16-­bit Unicode value|$0$|$65,535$|`\u0000`|`'a'`|
- **Some key points**
  - The `byte`, `short`, `int`, and `long` types are used for integer values
without decimal points.
  - Each numeric type uses twice as many bits as the smaller similar type.
   For example, `short` uses twice as many bits as `byte` does.
   - `short` is signed, which means it splits its range across the positive
    and negative integers. Alternatively, `char` is unsigned, which means
its range is strictly positive, including `0`.
  - All of the numeric types are signed and reserve one of their bits
  to cover a negative range. For example, instead of `byte` covering
  `0` to `255`, it actually covers `-128` to `127`.
  - You should have a general idea of the size of smaller types like `byte`
and `short`.
  - A `long` requires the letter `l` or `L` following the number so Java
knows it is a `long`. Without an `l` or `L`, Java interprets a number
without a decimal point as an `int` in most scenarios.
  - A `float` requires the letter `f` or `F` following the number so Java
knows it is a `float`. Without an `f` or `F`, Java interprets a decimal
value as a `double` (`12.3` is equal to `12.3d` or `12.3D`).