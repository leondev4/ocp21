---

markmap:
  initialExpandLevel: 2
---
# **Picking a Locale**
- The `Locale` class is in the `java.util` package. The first
 useful `Locale` to find is the user’s current locale.
  - ```java
    Locale locale = Locale.getDefault();
    System.out.println(locale);
    ```
    - When we run it, it prints `en_US`. It might be different for you. 
    This default output tells us that our computers are using
    **English** and are **sitting in the United States**.
- Notice the format.
  - Lowercase language code and is **always required**.
    Then comes an **underscore** followed by the 
    **uppercase optional country code**.
  - Locale
(language)
    - `en`
      - Lowercase 
        language
        code (required)
  - Locale
(language, country)
    - `en_US`
      - Lowercase 
        language
        code (required)
      - Uppercase
        country
        code
- Bad examples
  - ```js
    US    // Cannot have country without language
    enUS  // Missing underscore
    US_en // The country and language are reversed
    EN    // Language must be lowercase
    ```
    - The corrected versions are `en` and `en_US`.
# **Select a locale**
- You use the built-­in constants in the `Locale` 
class, available for some common locales.
  - ```js
    System.out.println(Locale.GERMAN);  // de
    System.out.println(Locale.GERMANY); // de_DE
    ```
    - The first example selects the German language
    - The second example selects both German 
      the language and Germany the country.
    - Only one includes a country code
- Use the `of` method of the `Locale` class. You can pass
  just a language, or both a language and country
  - ```js
    System.out.println(Locale.of("fr"));       // fr
    System.out.println(Locale.of("hi", "IN")); // hi_IN
    ```
    - The first is the language French, and the second is Hindi in India.
- The builder design pattern lets you set all of the properties that you 
care about and then build the Locale at the end. This means that 
you can specify the properties in any order.
  - ```js
    Locale l1 = 
      new Locale.Builder().setLanguage("en")
        .setRegion("US").build();

    Locale l2 = 
      new Locale.Builder().setRegion("US")
        .setLanguage("en").build();
    ```
    - The two `Locale` values both 
represent `en_US`
- Java will let you create a `Locale` with an invalid language or country, such as `xx_XX`. 
However, it will not match the Locale that you want to use, and your program will not 
behave as expected.
- Set a `Locale` other than your computer’s default.
  - ```js
    System.out.println(Locale.getDefault()); // en_US
    Locale locale = Locale.of("fr");
    Locale.setDefault(locale);
    System.out.println(Locale.getDefault()); // fr
    ```
  - ­The `Locale` changes for only that one Java program. It does not 
  change any settings on your computer. It does not even change 
  future executions of the same program.
# **Formatting Numbers**
- `NumberFormat` is in the package `java.text`.
  - Obtain an instance and then call `format()` to turn a number 
  into a `String`, or you can use `parse()` to turn a `String` into 
  a number.
    - The `NumberFormat.format()` method formats the
     given number based on the locale associated 
     with the `NumberFormat` object.
- General-­purpose formatter
  - ```js
    NumberFormat.getInstance() //default locale
    NumberFormat.getInstance(Locale locale)
    ```
    - ```js
      int attendeesPerYear = 3_200_000;
      int attendeesPerMonth = attendeesPerYear / 12;
                                        //en_US
      var us = NumberFormat.getInstance(Locale.US);
      System.out.println(us.format(attendeesPerMonth)); // 266,666
                                      //de_DE
      var gr =NumberFormat.getInstance(Locale.GERMANY);
      System.out.println(gr.format(attendeesPerMonth)); //266.666
                                      //fr_CA
      var ca =NumberFormat.getInstance(Locale.CANADA_FRENCH);
      System.out.println(ca.format(attendeesPerMonth)); //266 666
      ```
- Same as `getInstance`
  - ```
    NumberFormat.getNumberInstance()
    NumberFormat.getNumberInstance(Locale locale)
    ```
- For formatting monetary amounts
  - ```js
    NumberFormat.getCurrencyInstance()
    NumberFormat.getCurrencyInstance(Locale locale)
    ```
    - ```js
      double price = 48;
      var myLocale =NumberFormat.getCurrencyInstance();
      System.out.println(myLocale.format(price));
      ```
    - When run with the default locale of `en_US` for the United States, 
    this code outputs $48.00. 
    When run with the default locale of  `en_GB` for  Great Britain, it
    outputs £48.00.
- For formatting percentages
  - ```js
    NumberFormat.getPercentInstance()
    NumberFormat.getPercentInstance(Locale locale)
    ```
    - ```js
      double successRate = 0.802;
      var us = NumberFormat.getPercentInstance(Locale.US);
      System.out.println(us.format(successRate)); //80%

      var gr =NumberFormat.getPercentInstance(Locale.GERMANY);
      System.out.println(gr.format(successRate)); //80 %
      ```
- Rounds decimal values before displaying
  - ```js
    NumberFormat.getIntegerInstance()
    NumberFormat.getIntegerInstance(Locale locale)
    ```
- Returns compact number formatter
  - ```
    NumberFormat.getCompactNumberInstance()
    NumberFormat.getCompactNumberInstance(
    Locale locale, NumberFormat.Style formatStyle)
    ```