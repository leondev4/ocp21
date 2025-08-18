---
markmap:
  initialExpandLevel: 1
---
# **Accounting for Daylight Saving Time**
- The act of moving the clock forward or back occurs at 2:00
  a.m., which falls very early Sunday morning.
  [Figure 4.10](https://1drv.ms/i/c/c83cfca51d5c2032/EaKfHohBOAVGr3Rm7Rc_TU8BImpE-nHsp7H5_xuweOJEiA?e=9TwOLz) shows what happens with the clocks. When we
  change our clocks in March, time springs forward from
  1:59 a.m. to 3:00 a.m. When we change our clocks in
  November, time falls back, and we experience the hour
  from 1:00 a.m. to 1:59 a.m. twice.
  - For example, on March 9, 2025, we move our clocks
forward an hour and jump from 2:00 a.m. to 3:00 a.m. This
means that there is no 2:30 a.m. that day. If we wanted to
know the time an hour later than 1:30, it would be 3:30.
    - Example:
      ```java
      var date = LocalDate.of(2025, Month.MARCH, 9);
      var time = LocalTime.of(1, 30);
      var zone = ZoneId.of("US/Eastern");
      var zDT = ZonedDateTime.of(date, time, zone);

      System.out.println(zDT); // 2025–03-09T01:30-05:00[US/Eastern]
      System.out.println(zDT.getHour()); // 1
      System.out.println(zDT.getOffset()); // -05:00
      
      zDT =zDT.plusHours(1);
      System.out.println(zDT); // 2025–03-09T03:30-04:00[US/Eastern]
      System.out.println(zDT.getHour()); // 3
      System.out.println(zDT.getOffset()); // -04:00
      ```
      - Notice that two things change in this example. The time
      jumps from 1:30 to 3:30. The UTC offset also changes.
      Remember when we calculated GMT time by subtracting
      the time zone from the time? You can see that we went
      from 6:30 GMT (1:30 minus –5:00) to 7:30 GMT (3:30
      minus –4:00). This shows that the time really did change by
      one hour from GMT’s point of view. We printed the hour
      and offset fields separately for emphasis.
- Similarly, in November, an hour after the initial 1:30 a.m. is
also 1:30 a.m. because at 2:00 a.m. we repeat the hour.
This time, try to calculate the GMT time yourself for all
three times to confirm that we really do move only one hour
at a time.
  - Example:
    ```java
    var date = LocalDate.of(2025, Month.NOVEMBER, 2);
    var time = LocalTime.of(1, 30);
    var zone = ZoneId.of("US/Eastern");
    var zDT = ZonedDateTime.of(date, time, zone);
    System.out.println(zDT); // 2025-11-02T01:30-04:00[US/Eastern]
    zDT = zDT.plusHours(1);
    System.out.println(zDT); // 2025-11-02T01:30-05:00[US/Eastern]
    zDT = zDT.plusHours(1);
    System.out.println(zDT); // 2025-11-02T02:30-05:00[US/Eastern]
    ```

    - Finally, trying to create a time that doesn’t exist just rolls forward:
      ```java
      var date = LocalDate.of(2025, Month.MARCH, 9);
      var time = LocalTime.of(2, 30);
      var zone = ZoneId.of("US/Eastern");
      var zDT = ZonedDateTime.of(date, time, zone);
      System.out.println(zDT); // 2025–03–09T03:30–04:00[US/Eastern]
      ```
        - Java is smart enough to know that there is no 2:30 a.m. that
        night and switches over to the appropriate GMT offset.