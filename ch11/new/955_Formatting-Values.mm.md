---

markmap:
  initialExpandLevel: 2
---
# **Formatting Values**
- `NumberFormat` abstrac class
  - ```
    public final String format
    ```
    - ```
      (double number)
      ```
    - ```
      (long number)
      ```
- `DecimalFormat` extends `NumberFormat`
  -  Constructor:
      ```
      public DecimalFormat(String pattern)
      ```
- `DecimalFormat` symbols
  - `#`: Omit position if no digit exists for it.
    - format: `2.2`
      - `$###.##`    `$2.2` 
  - `0`: Put `0` in position if no digit exists for it.
    - format `2.2`
      - `$000.00`    `$002.20`
- ```js
  12: double d = 1234.567;
  13: NumberFormat f1 = new DecimalFormat("###,###,###.0");
  14: System.out.println(f1.format(d)); // 1,234.6
  15:
  16: NumberFormat f2 = new DecimalFormat("000,000,000.00000");
  17: System.out.println(f2.format(d)); // 000,001,234.56700
  18:
  19: NumberFormat f3 = new DecimalFormat("Your Balance $#,###,###.##");
  20: System.out.println(f3.format(d)); // Your Balance $1,234.57
  ```
  - Line 14 displays the digits in the number, rounding to the nearest 
  10th after the decimal. The extra positions to the left are omitted 
  because we used `#`. Line 17 adds leading and trailing zeros to 
  make the output the  desired length. Notice that the commas are 
  automatically removed if they are used between `#` symbols.
# **Formatting Dates and Times**
- Java provides a class called `DateTimeFormatter` 
to display standard formats.
- ```js
  var date = LocalDate.of(2024, Month.OCTOBER, 20);
  var time = LocalTime.of(11, 12, 34);
  var dt = LocalDateTime.of(date, time);
  var f1 = date.format(DateTimeFormatter.ISO_LOCAL_DATE);
  var f2 = time.format(DateTimeFormatter.ISO_LOCAL_TIME);
  var f3 = dt.format(DateTimeFormatter.ISO_LOCAL_DATE_TIME);
  System.out.println("%s%n%s%n%s".formatted(f1,f2,f3));
  ```
  - ```
    2024-10-20
    11:12:34
    2024-10-20T11:12:34
    ```
- The `DateTimeFormatter` will throw an exception 
if it encounters an incompatible type.
  - ```js
    date.format(DateTimeFormatter.ISO_LOCAL_TIME); // RuntimeException
    time.format(DateTimeFormatter.ISO_LOCAL_DATE); // RuntimeException
    ```
# **Customizing the Date/Time Format**
- `DateTimeFormatter` supports a custom
  format using a date format `String`.
- ```js
  var dt = LocalDateTime.of(2024, Month.OCTOBER, 20, 6, 15, 30);
  var f = DateTimeFormatter.ofPattern("MMMM dd, yyyy 'at' hh:mm");
  System.out.println(dt.format(f));//October 20, 2024 at 06:15
  ```
  - Java assigns each letter or symbol a specific date/time part. For example, `M` is used for 
  month, while `y` is used for year. And case matters! Using `m` instead of `M` means it will
  return the minute of the hour, not the month of the year.
  - Using `M` by itself outputs the minimum number of characters for a month, such as `1` 
  for January or `10` for october, while using `MM` always outputs two digits, such as `01`. 
  Furthermore, using `MMM` prints the three-­letter abbreviation, such as `Jul` for `July`, 
  while `MMMM` prints the full month name.
- Common date/time symbols
  |Symbol|Meaning|
  |-|-|
  |`y`|Year|
  |`M`|Month|
  |`d`|Day|
  |`H`|24 Hour|
  |`h`|12 Hour|
  |`m`|Minute|
  |`S`|Second|
  |`a`|a.m./p.m.|
  |`z`|Time zone name|
  |`Z`|Time zone offset|
  - As a tip, you can remember `H` is 24 hours and `h` is 12 hours 
  because uppercase is bigger. Similarly, `M` is month and `m` is 
  minute because `M` is bigger.
- ```js
  var dt = LocalDateTime.of(2022, Month.OCTOBER, 20, 6, 15, 30);

  var formatter1 = DateTimeFormatter.ofPattern("MM/dd/yyyy hh:mm:ss");
  System.out.println(dt.format(formatter1)); //10/20/2022 06:15:30

  //Underscore and hyphen are valid
  var formatter2 = DateTimeFormatter.ofPattern("MM_yyyy_-­_dd");
  System.out.println(dt.format(formatter2)); //10_2022_-­_20

  var formatter3 = DateTimeFormatter.ofPattern("h:mm z");
  System.out.println(dt.format(formatter3)); //DateTimeException
  ```
  - The _first example_ prints the date, with the month before the day, followed by the time.
  The _second example_ prints the date in a weird format with extra characters that are just 
  displayed as part of the output.
  The _third example_ throws an exception at runtime because the underlying `LocalDateTime` 
  does not have a time zone specified. If `ZonedDateTime` were used instead, the code would 
  complete successfully and print something like `06:15 EDT`, depending on the time zone.
- Make sure you know which symbols are compatible with which
 date/time types. For example, trying to format a month for a
  `LocalTime` or an hour for a `LocalDate` will result in a runtime 
  exception.
  - `LocalDate`
    - `y`,`M`,`d`
  - `LocalTime`
    - `h/H`,`m`,`s`,`a`
  - `LocalDateTime`
    - `y`,`M`,`d`
      `h/H`,`m`,`s`,`a`
  - `ZonedDateTime`
    - `y`,`M`,`d`
      `h/H`,`m`,`s`,`a`
      `z`,`Z`
- The date/time classes contain a `format()` method that will
 take a formatter, while the formatter classes contain a 
 `format()` method that will take a date/time value.
  - ```js
    var dateTime = LocalDateTime.of(2022, Month.OCTOBER, 20, 6, 15, 30);
    var formatter = DateTimeFormatter.ofPattern("MM/dd/yyyy hh:mm:ss");

    System.out.println(dateTime.format(formatter)); //10/20/2022 06:15:30
    System.out.println(formatter.format(dateTime)); //10/20/2022 06:15:30
    ```

    These statements print the same value at runtime. 
# **Adding Custom Text Values**
- You can _escape_ the text by surrounding it with a pair of single quotes (`'`). 
Escaping text instructs the formatter to ignore the values inside the single 
quotes and just insert them as part of the final value.
  - ```js
    var f = DateTimeFormatter.ofPattern("MMMM dd, yyyy 'at' hh:mm");
    System.out.println(dt.format(f)); // October 20, 2022 at 06:15
    ```
- But what if you need to display a single quote in the output, too? 
Welcome to the fun of escaping characters! Java supports this
by putting two single quotes next to each other. 
  - ```js
    var g1 = DateTimeFormatter.ofPattern("MMMM dd', Party''s at' hh:mm");
    System.out.println(dt.format(g1)); //October 20, Party's at 06:15
    ```
  - ```js
    var g2 = DateTimeFormatter.ofPattern("'System format, hh:mm: 'hh:mm");
    System.out.println(dt.format(g2)); //System format, hh:mm: 06:15
    ```
  - ```js
    var g3 = DateTimeFormatter.ofPattern("'NEW! 'yyyy', yay!'");
    System.out.println(dt.format(g3)); // NEW! 2022, yay!`

  - If you don’t escape the text values with single quotes, an exception will be thrown at runtime if 
  the text cannot be interpreted as a date/time symbol.

    ```js
    DateTimeFormatter.ofPattern("The time is hh:mm"); // IllegalArgumentException
    ```

    This line throws an exception since `T` is an unknown symbol. The exam might also present you 
    with an incomplete escape sequence.

    ```js
    DateTimeFormatter.ofPattern("'Time is: hh:mm: "); // IllegalArgumentException
    ```