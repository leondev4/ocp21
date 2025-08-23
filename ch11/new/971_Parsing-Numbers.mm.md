---

markmap:
  initialExpandLevel: 2
---
# **Parsing Numbers**
- We convert it from a `String` to a structured object or 
primitive value, using the `NumberFormat.parse()`
method; and takes the locale into consideration.
  - The `parse()` method, declares a checked exception
   `ParseException` that _must be handled or declared_
- ```js
  var en = NumberFormat.getInstance(Locale.US);
  System.out.println(en.parse(s)); // 40.45

  var fr = NumberFormat.getInstance(Locale.FRANCE);
  System.out.println(fr.parse(s)); // 40
  ```
  - In the United States, a dot (`.`) is part of a number, and the number is 
  parsed as you might expect. France does not use a decimal point to 
  separate numbers. Java parses it as a formatting character, and it 
  stops looking at the rest of the number.
- ```js
  String s = "40a45";
  var en = NumberFormat.getInstance(Locale.US);
  System.out.println(en.parse(s)); //40

  var fr = NumberFormat.getInstance(Locale.FRANCE);
  System.out.println(fr.parse(s)); //40
  ```
  - ```js
    String s = "a4045";
    //both throw ParseException
    var en = NumberFormat.getInstance(Locale.US);
    System.out.println(en.parse(s));  

    var fr = NumberFormat.getInstance(Locale.FRANCE);
    System.out.println(fr.parse(s)); 
    ```
- The `parse()` method is also used for parsing currency.
  ```js
  String income = "$92,807.99";
  var cf = NumberFormat.getCurrencyInstance();
  double value = (Double)cf.parse(income);
  System.out.println(value); // 92807.99
  ```
  - The currency string `"$92,807.99"` contains a dollar sign and a 
  comma. The `parse` method strips out the characters and converts 
  the value to a number. The return value of `parse` is a `Number` 
  object. So it can be cast to its appropriate data type. The `Number`
  is cast to a `Double` and then automatically unboxed into a `double`.
# **Formatting with `CompactNumberFormat`**
- The second class that inherits `NumberFormat` 
(the first is `DecimalFormat`) that you need to 
know for the exam is `CompactNumberFormat`.
  - It is designed to be used in places
  where print space may be limited.
- Rules for `CompactNumberFormat`:
  - First it determines the highest range for the 
  number, such as thousand (`K`), million (`M`),
  billion (`B`), or trillion (`T`).
  - It then returns up to the first three digits of that
  range, rounding the last digit as needed.
  - Finally, it prints an identifier. If `SHORT` is used, 
  a symbol is returned. If `LONG` is used, a space 
  followed by a word is returned.
- ```js
  var formatters = Stream.of(
    NumberFormat.getCompactNumberInstance(),
    NumberFormat.getCompactNumberInstance(Locale.getDefault(), Style.SHORT),
    NumberFormat.getCompactNumberInstance(Locale.getDefault(), Style.LONG),
    NumberFormat.getCompactNumberInstance(Locale.GERMAN, Style.SHORT),
    NumberFormat.getCompactNumberInstance(Locale.GERMAN, Style.LONG),
    NumberFormat.getNumberInstance());

  formatters.map(s ->s.format(7_123_456)).forEach(System.out::println);
  ```
  - The first two lines are the same. If you don’t specify a 
style, `SHORT` is used by default, similarly for `Locale`.
  - When you run it in `en_US`.
    ```
    7M
    7M
    7 million

    77Mio.
    Millionen

    7,123,456
    ```
- ```
  formatters.map(s -­> s.format(314_900_000)).forEach(System.out::println);
  ```
  - When you run it in `en_US`.
    ```
    315M
    315M
    315 million

    315315Mio.
    Millionen

    314,900,000
    ```
- `CompactNumberFormat` can also be used for parsing, 
although not always in the way you might expect!
  - ```js
    20: var locale = Locale.of("en", "US");
    21: var compact = NumberFormat.getCompactNumberInstance(
    22:   locale, Style.SHORT);
    23: System.out.println(compact.format(1_000_000)); // 1M
    24: System.out.println(compact.parse("1M"));       // 1000000
    25: System.out.println(compact.parse("1000000"));  // 1000000
    26: System.out.println(compact.parse("1,000,000"));// 1
    27: System.out.println(compact.parse("$1000000")); // ParseException
    ```
    - Lines 20-23 should look familiar. They print a million using the
    short format of `1M`. Lines 24 and 25 shows that the format is
    flexible in taking the original or shortened format. Line 26
    might surprise you as Java stops at the initial punctuation and
    only prints `1`. By contrast, line 27 is a road too far. Java doesn’t
    know what to do with the `$` and throws a `ParseException`.