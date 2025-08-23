---

markmap:
  initialExpandLevel: 2
---
# **Localizing Dates**
- Factory methods to get 
  a `DateTimeFormatter`
  - For formatting dates
    - ```js
      DateTimeFormatter.ofLocalizedDate(FormatStyle dateStyle)
      ```
  - For formatting times
    - ```
      DateTimeFormatter.ofLocalizedTime(FormatStyle timeStyle)
      ```
  - For formatting dates and
times
    - ```
      DateTimeFormatter.ofLocalizedDateTime(FormatStyle dateStyle, FormatStyle timeStyle)
      DateTimeFormatter.ofLocalizedDateTime(FormatStyle dateTimeStyle)
      ```
  - Each method takes a `FormatStyle` parameter (or two) with possible values `SHORT`, `MEDIUM`, `LONG`, and `FULL`.
- What if you need a formatter for a specific locale? Easy enough—­just append
`withLocale(locale)` to the method call.
- ```js
  public static void print(DateTimeFormatter dtf,
    LocalDateTime dateTime, Locale locale) {
    System.out.println(dtf.format(dateTime) + " -­-­-­"
    + dtf.withLocale(locale).format(dateTime));
  }
  public static void main(String[] args) {
    Locale.setDefault(Locale.of("en", "US"));
    var italy = Locale.of("it", "IT");
    var dt = LocalDateTime.of(2022, Month.OCTOBER, 20, 15, 12, 34);

    // 10/20/22 -­-­-­ 20/10/22
    print(DateTimeFormatter.ofLocalizedDate(SHORT),dt,italy);

    // 3:12 PM -­-­-­ 15:12
    print(DateTimeFormatter.ofLocalizedTime(SHORT),dt,italy);

    // 10/20/22, 3:12 PM -­-- 20/10/22, 15:12
    print(DateTimeFormatter.ofLocalizedDateTime(SHORT,SHORT),dt,italy);
  }
  ```
  - First we establish `en_US` as the default locale, with `it_IT` as the requested 
  locale. We then output each value using the two locales. As you can see, 
  applying a locale has a big impact on the built-­in date and time formatters.
# **Specifying a Locale Category**
- You use `Locale.setDefault(Locale.Category category, Locale newLocale)`
- The `Locale.Category` enum is a nested element in 
`Locale` that supports distinct locales for _displaying_ 
and _formatting_ data.
  - `Locale.Category` values
    - `DISPLAY`
      - Category used for displaying data about locale
    - `FORMAT`
      - Category used for formatting dates, numbers, or currencies
- When you call **`Locale.setDefault()` with a locale, 
the `DISPLAY` and `FORMAT` are set together.** 
- ```js
  10: public static void printCurrency(Locale locale, double money) {
  11:   System.out.println(
  12:      NumberFormat.getCurrencyInstance().format(money)
  13:      + ", " + locale.getDisplayLanguage());
  14: }
  15: public static void main(String[] args) {
  16:   var spain = Locale.of("es", "ES");
  17:   var money = 1.23;
  18:
  19:    // Print with default locale
  20:    Locale.setDefault(Locale.of("en", "US"));   //DISPLAY:en_US
  21:    printCurrency(spain, money); // $1.23, Spanish//FORMAT:en_US
  22:
  23:     // Print with selected locale display
  24:     Locale.setDefault(Category.DISPLAY, spain); //DISPLAY:es_ES
  25:     printCurrency(spain, money); // $1.23, español //FORMAT:en_US
  26:
  27:    // Print with selected locale format
  28:    Locale.setDefault(Category.FORMAT, spain); //DISPLAY:es_ES
  29:    printCurrency(spain, money); // 1,23 €, español //FORMAT:es_ES
  30: }
  ```
  - The code prints the same data three times. First it prints the language of the 
  spain and money variables using the locale `en_US`. Then it prints it using 
  the `DISPLAY` category of `es_ES`, while the `FORMAT` category remains 
  `en_US`. Finally, it prints the data using both categories set to `es_ES`.
  - You just need to know that you can set parts of the locale independently.
You should also know that calling `Locale.setDefault(us)` after the previous 
code snippet will **change both locale categories to `en_US`**.