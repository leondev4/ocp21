---
markmap:
  initialExpandLevel: 1
---
# **Working with Periods**

- Our zoo performs animal enrichment activities to give the
animals something enjoyable to do. The head zookeeper
has decided to switch the toys every month. This system
will continue for three months to see how it works out.

  - Java has a `Period` class that we can pass in.
    ```java
      public static void main(String[] args) {
        var start = LocalDate.of(2025, Month.JANUARY, 1);
        var end = LocalDate.of(2025, Month.MARCH, 30);
        var period = Period.ofMonths(1); // create a period
        activities(start, end, period);
      }
      private static void activities(LocalDate start, LocalDate end,
          Period period) { // uses the generic period
        var upTo = start;
        while (upTo.isBefore(end)) {
          System.out.println("give new toy: " + upTo);
          upTo = upTo.plus(period); // adds the period
      } }
    ```
      - The method can add an arbitrary period of time that is
      passed in. This allows us to reuse the same method for
      different periods of time.
      - A `Period` can be positive (forward in time) or negative (backwards in time.) There are 
      five ways to create a `Period` class.
        ```java
        var annually = Period.ofYears(1); // every 1 year
        var quarterly = Period.ofMonths(3); // every 3 months
        var everyThreeWeeks = Period.ofWeeks(-3); // every 3 weeks going backwards
        var everyOtherDay = Period.ofDays(2); // every 2 days
        var everyYearAndAWeek = Period.of(1, 0, 7); // every year plus 1 week
        ```
        -  **<span style="color:#008000">You cannot chain methods when creating a `Period`.</span>** The following
         code looks like it is equivalent to the `everyYearAndAWeek` example, but
          it’s not. **<span style="color:#008000">Only the last method is used because the methods are 
          `static` methods.**
            ```java
            var wrong = Period.ofYears(1).ofWeeks(1); // every week
            ```
            - This tricky code is really like writing the following:
              ```java
              var wrong = Period.ofYears(1);
              wrong = Period.ofWeeks(1);
              ```
              This is clearly not what you intended! That’s why the `of()`
              method allows you to pass in the number of years, months,
              and days. They are all included in the same period.

              - The `of()` method takes only years, months, and days. The
              ability to use another factory method to pass weeks is
              merely a convenience. As you might imagine, the actual
              period is stored in terms of years, months, and days. When
              you print out the value, **Java displays any nonzero parts**
              using the format shown in [Figure 4.9](https://1drv.ms/i/c/c83cfca51d5c2032/EffqyKX6S4lGhK-qZBqf9DkBs9vcLo6LPHShUtyTlDY2_g?e=14OQMr).
                - As you can see, the `P` always starts out the `String` to show it
                is a period measure. Then come the number of years, number 
                of months, and number of days. **If any of these are zero, they 
                are omitted.**
- Can you figure out what this outputs?
  ```java
  System.out.println(Period.ofMonths(3));
  ```
  The output is `P3M`. Remember that Java omits 
  any measures that are zero.

- You can also create a period by getting the amount of
 time between two `LocalDate` objects:
  ```java
  var oldDate = LocalDate.of(2025, Month.DECEMBER, 25);
  var newDate = LocalDate.of(2026, Month.JANUARY, 1);
  // normal date order, the result is positive
  var p1 = Period.between(oldDate, newDate); // P7D
  var p2 = Period.between(newDate, oldDate); // P-7D
  ```
  - Notice how order matters. The first time `Period.between()`
  returns a period representing seven days, but the second
  time it returns a period of negative seven days.
    - The last thing to know about `Period` is what objects it can be used 
    with. Let’s look at some code:
      ```java
      3: var date = LocalDate.of(2025, 3, 20);
      4: var time = LocalTime.of(6, 15);
      5: var dateTime = LocalDateTime.of(date, time);
      6: var period = Period.ofMonths(-1);
      7: var p1 = date.plus(period);     // 2025–02–20
      8: var p2 = dateTime.plus(period); // 2025–02–20T06:15
      9: var p3 = time.plus(period);     // Exception
      ```
      You can use `Period` with `LocalDate`, `LocalDateTime` and
       `ZonedDateTime`.
       - Lines 7 and 8 work as expected. They subtract a month
      from March 20, 2025, giving us February 20, 2025. The
      first has only the date, and the second has both the date
      and time.
      Line 9 attempts to add a month to an object that has only 
      a time. This won’t work. Java throws an
      `UnsupportedTemporalTypeException` and complains that we
      attempted to use an `Unsupported unit: Months`.
- **Working with Durations**
You’ve probably noticed by now that a `Period` is a day or
more of time. There is also `Duration`, which is intended for
smaller units of time. For `Duration`, you can specify the
number of days, hours, minutes, seconds, or nanoseconds.
You can use `Duration` with `LocalTime`, `LocalDateTime` and 
`ZonedDateTime`.
  - Conveniently, `Duration` works roughly the same way as
  `Period`, except it is used with objects that have time. `Duration`
  is output beginning with `PT`, which you can think of as a period
   of time. A `Duration` is stored in hours, minutes, and seconds. 
   The number of seconds includes fractional seconds.
    - We can create a `Duration` using a number of different granularities:
      ```java
      var twoDays = Duration.ofDays(2); // PT48H
      var hourly = Duration.ofHours(1); // PT1H
      var everyMinute = Duration.ofMinutes(1); // PT1M
      var everyTenSeconds = Duration.ofSeconds(10); // PT10S
      var everyMilli = Duration.ofMillis(1); // PT0.001S
      var everyNano = Duration.ofNanos(1); // PT0.000000001S
      ```
      - `Duration` doesn’t have a factory method that takes multiple
      units like `Period` does. If you want something to happen
      every hour and a half, you specify 90 minutes.
        - `Duration` includes another more generic factory method. It takes a 
        number and a `TemporalUnit`. The idea is, say, something like “5 
        seconds.” However, `TemporalUnit` is an interface. At the moment,
         there is only one implementation named `ChronoUnit`.
          ```java
          public static Duration of(long amount, TemporalUnit unit)
          ```
            - The previous example could be rewritten like this:
              ```java 
              var twoDays = Duration.of(2, ChronoUnit.DAYS);
              var hourly = Duration.of(1, ChronoUnit.HOURS);
              var everyMinute = Duration.of(1, ChronoUnit.MINUTES);
              var everyTenSeconds = Duration.of(10, ChronoUnit.SECONDS);
              var everyMilli = Duration.of(1, ChronoUnit.MILLIS);
              var everyNano = Duration.of(1, ChronoUnit.NANOS);
              ```
              `ChronoUnit` also includes some convenient units such as
              `ChronoUnit.HALF_DAYS` to represent 12 hours.
- **ChronoUnit for Differences**
`ChronoUnit` is a great way to determine how far apart two `Temporal` values are.
`Temporal` includes `LocalDate`, `LocalTime`, and so on. `ChronoUnit` is in the 
`java.time.temporal` package.
  ```java
  var one = LocalTime.of(5, 15);
  var two = LocalTime.of(6, 55);
  var date = LocalDate.of(2025, 1, 20);
  var h = ChronoUnit.HOURS.between(one, two); // 1
  var m = ChronoUnit.MINUTES.between(one, two); // 100
  var e = ChronoUnit.MINUTES.between(one, date); // DateTimeException
  ```
  - The first variable `h` shows that `between` truncates rather 
  than rounds. The second shows how easy it is to count
  in different units. Just change the `ChronoUnit` type. The
  last reminds us that Java will throw an exception if we
  mix up what can be done on date versus time objects.
    - Alternatively, you can truncate any object with a time element. 
    For example:
      ```java
      LocalTime time = LocalTime.of(3,12,45);
      System.out.println(time); // 03:12:45
      LocalTime truncated = time.truncatedTo(ChronoUnit.MINUTES);
      System.out.println(truncated); // 03:12
      ```
      This example zeroes out any fields smaller than minutes. In our case, 
      it gets rid of the seconds.
        - Using a `Duration` works the same way as using a `Period`. For example:
          ```java
          7:  var date = LocalDate.of(2025, 1, 20);
          8:  var time = LocalTime.of(6, 15);
          9:  var dateTime = LocalDateTime.of(date, time);
          10: var duration = Duration.ofHours(6);

          11: var r1 = dateTime.plus(duration); // 2025–01–20T12:15
          12: var r2 = time.plus(duration);     // 12:15
          13: var r3 = date.plus(duration);
          14:     // UnsupportedTemporalTypeException
          ```
          - Line 11 shows that we can add hours to a `LocalDateTime`,
          since it contains a time. Line 12 also works, since all we
          have is a time. Line 13 fails because we cannot add hours
          to an object that does not contain a time.
            - Let’s try that again, but add 23 hours this time.
              ```java
              7:  var date = LocalDate.of(2025, 1, 20);
              8:  var time = LocalTime.of(6, 15);
              9:  var dateTime = LocalDateTime.of(date, time);
              10: var duration = Duration.ofHours(23);

              11: var r1 = dateTime.plus(duration); // 2025–01–21T05:15
              12: var r2 = time.plus(duration); // 05:15
              13: var r3 = date.plus(duration);
              14:    // UnsupportedTemporalTypeException
              ```
            - This time we see that Java moves forward past the end of
the day. Line 11 goes to the next day since we pass
midnight. Line 12 doesn’t have a day, so the time just
wraps around—just like on a real clock.
- **Period vs. Duration**
Remember that `Period` and `Duration` are not equivalent. This
example shows a `Period` and `Duration` of the same length:
  ```java
  var date = LocalDate.of(2025, 5, 25);
  var day = Period.ofDays(1);//P1D
  var hours = Duration.ofDays(1);//PT24H
  var r1 = date.plus(day); // 2025–05–26
  var r2 = date.plus(hours); // Unsupported unit: Seconds
  ```
  - Since we are working with a `LocalDate`, we are required to
  use `Period`. `Duration` has time units in it, even if we don’t see
  them, and they are meant only for objects with time.
    - You can use `Period` with
      - `LocalDate`
        `LocalDateTime`
        `ZonedDateTime` 
    - You can use `Duration` with
      - `LocalDateTime`
        `LocalTime` 
        `ZonedDateTime`
  - **Working with Instants**
The `Instant` class represents a specific moment in time in
the GMT time zone. Suppose that you want to run a timer.
    ```java
    var now = Instant.now();
    // Do something time consuming
    var later = Instant.now();
    var duration = Duration.between(now, later);
    var m = duration.toMillis(); //Returns milliseconds
    ```
    - In our case, the “something time consuming” was just over
    a second, and `m` contains `1025`.
      - If you have a `ZonedDateTime`, you can turn it into an `Instant`:
        ```java
        var date = LocalDate.of(2025, 5, 25);
        var time = LocalTime.of(11, 55, 00);
        var zone = ZoneId.of("US/Eastern");
        var zDT = ZonedDateTime.of(date, time, zone);

        var instant = zDT.toInstant(); // 2025–05–25T15:55:00Z
        println(zonedDateTime); // 2025–05–25T11:55–04:00[US/Eastern]
        println(instant);       // 2025–05–25T15:55:00Z
        ```
        - The last two lines represent the same moment in time. The
`ZonedDateTime` includes a time zone. **The `Instant` gets rid of
the time zone and turns it into an `Instant` of time in GMT**.
          - **You cannot convert a `LocalDateTime` to an `Instant`.** Remember
that an `Instant` is a point in time. A `LocalDateTime` does not contain 
a time zone, and it is therefore not universally recognized around
 the world as the same moment in time.