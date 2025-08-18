---

markmap:
  initialExpandLevel: 1
---
# **Working with Dates and Times**
- You need an import statement to work with the
 modern date and time classes.
  ```
  import java.time.*; // import time classes
  ```
  - **Creating Dates and Times**
  When working with dates and times, the first thing to do is
  to decide how much information you need. We have four
  choices.
    - **`LocalDate`** Contains just a date—no time and no time
    zone. A good example of `LocalDate` is your birthday this
    year. It is your birthday for a full day, regardless of
    what time it is.
    - **`LocalTime`** Contains just a time—no date and no time
    zone. A good example of `LocalTime` is midnight. It is
    midnight at the same time every day.
    - **`LocalDateTime`** Contains both a date and time but no
    time zone. A good example of `LocalDateTime` is “the
    stroke of midnight on New Year’s Eve.”
    - **`ZonedDateTime`** Contains a date, time, and time zone. A
    good example of `ZonedDateTime` is “a conference call at 9
    a.m. EST.” If you live in California, you’ll have to get up
    really early since the call is at 6 a.m. local time!

- You obtain date and time instances using a `static` method.
  ```java
  System.out.println(LocalDate.now());
  System.out.println(LocalTime.now());
  System.out.println(LocalDateTime.now());
  System.out.println(ZonedDateTime.now());
  ```
  Each of the four classes has a `static` method called `now()`,
  which gives the current date and time. Your output is going
  to depend on the date/time when you run it and where you
  live. The authors live in the United States, making the output 
  look like the following when run on `July 25` at    `9:13
  a.m.`:
  - ```java
    2025–07–25
    09:13:07.768
    2025–07–25T09:13:07.768
    2025–07–25T09:13:07.769–04:00[America/New_York]
    ```
    - The key is the type of information in the output. The first line 
    contains only a date and no time. The second contains only a 
    time and no date. The time displays hours, minutes,    seconds,
    and fractional seconds. The third contains both a date and a 
    time. The output uses `T` to separate the date and     time when 
    converting `LocalDateTime` to a `String`. Finally, the fourth 
    adds the time zone offset and time zone.
      - Greenwich Mean Time is a time zone in Europe that is used
      as time zone zero when discussing offsets. You might have
      also heard of Coordinated Universal Time, which is a time
      zone standard. It is abbreviated as UTC, uses the same time
      zone zero as GMT.
        - First, let’s try to figure out how far apart the following moments are in time. 
        Notice how India has a half-hour offset, not a full hour. To approach a
         problem like this, **you subtract the time zone from the time**. This gives 
         you the GMT equivalent of the time:
          ```java
          2025–06–20T06:50+05:30[Asia/Kolkata] // GMT 2025–06–20 01:20
          2025–06–20T07:50-04:00[US/Eastern] // GMT 2025–06–20 11:50
          ```
          Remember that you need to add when subtracting a negative number. 
          After converting to GMT, you can see that the U.S. Eastern time occurs
           10 and a half hours after           the Kolkata time.
           - The time zone offset can be listed in different ways:
          `+02:00`, `GMT+2`, and `UTC+2` all mean the same thing.
          You might see any of them on the exam.
- Both of these examples create the same date:
  ```java
  var date1 = LocalDate.of(2025, Month.JANUARY, 20);
  var date2 = LocalDate.of(2025, 1, 20);
  ```
  Both pass in the year, month, and date. Although it is good
  to use the `Month` constants, you can pass the `int` number
  of the month directly. Just use the number of the month the
  same way you would if you were writing the date in real life.
  - The method signatures are as follows:
    ```java
    public static LocalDate of(int year, int month, int dayOfMonth)
    public static LocalDate of(int year, Month month, int dayOfMonth)
    ```
    For months in the new date and time methods, Java counts starting from 1, just
     as we humans do.
     - When creating a time, you can choose how detailed you want to be.
      You can specify just the hour and minute, or you can include the 
      number of seconds. You can even include nanoseconds if you want
       to be very precise.    
        ```java
        var time1 = LocalTime.of(6, 15);        // hour and minute
        var time2 = LocalTime.of(6, 15, 30);    // + seconds
        var time3 = LocalTime.of(6, 15, 30, 200); // + nanoseconds
        ```
        - These three times are all different but within a minute of each other. The method
         signatures are as follows:
          ```java
          public static LocalTime of(int hour, int minute)
          public static LocalTime of(int hour, int minute, int second)
          public static LocalTime of(int hour, int minute, int second, int nanos)
          ```

          - You can combine dates and times into one object.
            ```java
            var dateTime1 = LocalDateTime.of(2025, Month.JANUARY, 20, 6,15, 30);
            var dateTime2 = LocalDateTime.of(date1, time2);
            ```
            The first line of code shows how you can specify all of the information about the
             `LocalDateTime` right in the same line. The second line of code shows how you can
              create `LocalDate` and `LocalTime` objects separately first and then combine them to 
              create a `LocalDateTime` object.
            - There are a lot of method signatures since there are more combinations.  
            The following method signatures use integer values:
              ```java
              public static LocalDateTime of(int year, int month,
                int dayOfMonth, int hour, int minute)
              public static LocalDateTime of(int year, int month,
                int dayOfMonth, int hour, int minute, int second)
              public static LocalDateTime of(int year, int month,
                int dayOfMonth, int hour, int minute, int second, int nanos)
              ```
            - Others take a `Month` reference:
              ```java
              public static LocalDateTime of(int year, Month month,
                int dayOfMonth, int hour, int minute)
              public static LocalDateTime of(int year, Month month,
                int dayOfMonth, int hour, int minute, int second)
              public static LocalDateTime of(int year, Month month,
                int dayOfMonth, int hour, int minute, int second, int nanos)
              ```
            - Finally, one takes an existing `LocalDate` and `LocalTime`:
              ```java
              public static LocalDateTime of(LocalDate date, LocalTime time)
              ```
- **create a `ZonedDateTime`**
To create a `ZonedDateTime`, we first need to get the desired
time zone.
  ```java
  var zone = ZoneId.of("US/Eastern");
  var zoned1 = ZonedDateTime.of(2025, 1, 20, 6, 15, 
              30, 200, zone);
  var zoned2 = ZonedDateTime.of(date1, time1, zone);
  var zoned3 = ZonedDateTime.of(dateTime1, zone);
  ```
  - We start by getting the time zone object. Then we use one
  of three approaches to create the `ZonedDateTime`. The first
  passes all of the fields individually. A better approach is to
   pass a `LocalDate` object and a `LocalTime` object, or a 
   `LocalDateTime` object.

    - Although there are other ways of creating a `ZonedDateTime`, you only need to know three
      ```java
      public static ZonedDateTime of(int year, int month, int dayOfMonth,
          int hour, int minute, int second, int nanos, ZoneId zone)
      public static ZonedDateTime of(LocalDate date, LocalTime time,
          ZoneId zone)
      public static ZonedDateTime of(LocalDateTime dateTime, ZoneId zone)
      ```
      Notice that there isn’t an option to pass in the `Month` enum.

      - The date and time classes have private constructors along
      with `static` methods that return instances. This is known as
      the factory pattern.
        ```java
        var d = new LocalDate(); // DOES NOT COMPILE
        ```
        Don’t fall for this. You are not allowed to construct a  date or
        time object directly.
        - Another trick is what happens when you pass invalid numbers to `of()`, for example:
          ```java
          var d = LocalDate.of(2025, Month.JANUARY, 32); // DateTimeException
          ```
          You don’t need to know the exact exception that’s thrown, but it’s a clear one:
          ```java
          java.time.DateTimeException: Invalid value for DayOfMonth
                (valid values 1-28/31): 32
          ```
- **Manipulating Dates and Times**
Adding to a date is easy. The date and time classes are
immutable. Remember to assign the results of these
methods to a reference variable so they are not lost.
  - Example:
    ```java
    12: var date = LocalDate.of(2025, Month.JANUARY, 20);
    13: System.out.println(date); // 2025–01–20
    14: date = date.plusDays(2);
    15: System.out.println(date); // 2025–01–22
    16: date = date.plusWeeks(1);
    17: System.out.println(date); // 2025–01–29
    18: date = date.plusMonths(1);
    19: System.out.println(date); // 2025–02–28
    20: date = date.plusYears(5);
    21: System.out.println(date); // 2030–02–28
    ```
    - There are also nice, easy methods to go backward in time. This time,
     let’s work with `LocalDateTime`:
      ```java
      22: var date = LocalDate.of(2025, Month.JANUARY, 20);
      23: var time = LocalTime.of(5, 15);
      24: var dateTime = LocalDateTime.of(date, time);
      25: System.out.println(dateTime); // 2025–01–20T05:15
      26: dateTime = dateTime.minusDays(1);
      27: System.out.println(dateTime); // 2025–01–19T05:15
      28: dateTime = dateTime.minusHours(10);
      29: System.out.println(dateTime); // 2025–01–18T19:15
      30: dateTime = dateTime.minusSeconds(30);
      31: System.out.println(dateTime); // 2025–01–18T19:14:30
      ```
      - You can see that all of a sudden, the display value starts 
      showing seconds. Java is smart enough to hide the
       seconds and nanoseconds when we aren’t using them.
      It is common for date and time methods to be chained.
       The previous example could be rewritten as follows: 
         ```java
        var dateTime = LocalDateTime.of(date, time)
          .minusDays(1).minusHours(10).minusSeconds(30);
         ```
        - There are two ways that the exam
        creators can try to trick you.
          - What do you think this prints?
            ```java
            var date = LocalDate.of(2025, Month.JANUARY, 20);
            date.plusDays(10);
            System.out.println(date);
            ```
            - It prints `2025-01-20`. Adding 10 days was useless because the
            program ignored the result. Whenever you see immutable
            types, pay attention to make sure that the return value of a
            method call isn’t ignored. 
          -  Do you see what is wrong here?
              ```java
              var date = LocalDate.of(2025, Month.JANUARY, 20);
              date = date.plusMinutes(1); // DOES NOT COMPILE
              ```
               - `LocalDate` does not contain time. This means you cannot add
              minutes to it. This can be tricky in a chained sequence of
              addition/subtraction operations, so make sure you know
              which methods in **Table 4.6** can be called on which types.
- **TABLE 4.6** Methods in `LocalDate`, `LocalTime`, `LocalDateTime`, and `ZonedDateTime`
  |You can call on|Years/hours|Months/minutes|Weeks/seconds|Days/nanos|
  |-|-|-|-|-|
  |`LocalDate`<br>`LocalDateTime`<br/>`ZonedDateTime`|`plusYears()`<br/>`minusYears()`<br>`withYear()`<br>`withDayOfYear()`|`plusMonths()`<br/>`minusMonths()`<br/>`withMonth()`|`plusWeeks()`<br/>`minusWeeks()`|`plusDays()`<br/>`minusDays()`<br/>`withDayOfMonth()`|
  |`LocalTime`<br>`LocalDateTime`<br/>`ZonedDateTime`|`plusHours()`<br/>`minusHours()`<br/>`withHour()`|`plusMinutes()`<br/>`minusMinutes()`<br/>`withMinute()`|`plusSeconds()`<br/>`minusSeconds()`<br/>`withSecond()`|`plusNanos()`<br/>`minusNanos()`<br/>`withNano()`|
    - **Table 4.6** also includes methods that you can use to create a copy of an 
    object with specific field(s) altered to the     specified value. For example:
      ```java
      var date = LocalDate.of(2025, Month.FEBRUARY, 20); // 2025-02-20
      var differentDay = date.withDayOfMonth(15); // 2025-02-15
      var differentMonth = date.withDayOfYear(3); // 2025-01-03
      var allChanged = date.withYear(2026)
          .withMonth(4).withDayOfMonth(10); // 2026-04-10
      ```
      - Finally, there are methods to convert from one type to another. 
      For example:
        ```java
        var date = LocalDate.of(2025, Month.MARCH, 3);
        var withTime = date.atTime(5, 30); // 2025-03-03T05:30
        var start = date.atStartOfDay(); // 2025-03-03T00:00
        ```
        - The `at_________()` methods combine the instance variable and the 
        parameter into one new object. They are listed in **Table 4.7**.
        - **TABLE 4.7** Conversion methods in `LocalDate`, `LocalTime`, and
        `LocalDateTime`
          |Conversion|Methods|
          |-|-|
          |`LocalDate` **to** `LocalDateTime`|`atStartOfDay()`<br/>`atTime(int hour, int minute)`<br/>`atTime(int h, int m, int second)`<br/>`atTime(int h, int m,int s, int nanos)`<br/>`atTime(LocalTime time)` <br/> <br/>|
          |`LocalTime` **to** `LocalDateTime`|`atDate(LocalDate date)`|
          |`LocalDateTime` **to**<br/>`ZonedDateTime`|`atZone(ZoneId zoneId)`|