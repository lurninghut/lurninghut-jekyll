---
layout: post
title:  "Java LocalDateTime vs ZonedDateTime"
author: Gaurav
categories: [ Java ]
---
Very often we code applications in the real world where we need to store a date
along with the time. For e.g. Creating an application for Astrology where we want to 
store the date of birth and time for a person. Or, an appointment application for a pet clinic
where we want to store the date and time for an appointment. Do we see any need of asking timezone 
as a data point to be captured? No.

Now imagine you are coding an application for an International sports firm who wants to cover 
all the events happening around the different parts of the world. Do you see a need of knowing where 
exactly the event is happening so that we can use that locale information to show the correct event 
date or time in the UI based on where the webpage is being viewed. Here we should use ZonedDateTime object
to capture the timezone information as well.

The following will give you local date time shown on your system clock but it will not capture any
timezone information.
~~~java
LocalDateTime localDateTime = LocalDateTime.now();
~~~

### Convert to ZonedDateTime
We need to add zone id information to the LocalDateTime object, to get the ZonedDateTime object.
~~~java
ZonedDateTime zonedDateTime = localDateTime.atZone(ZoneId.of("Australia/Sydney"));
~~~

Here is a test code snippet for more clarity:
~~~java
import java.time.LocalDateTime;
import java.time.ZoneId;
import java.time.ZonedDateTime;

public class LocalDateTimeTest {
    public static void main(String[] args) {
        LocalDateTime localDateTime = LocalDateTime.now();
        ZonedDateTime zonedDateTime = localDateTime.atZone(ZoneId.of("Australia/Sydney"));
        System.out.println(localDateTime.getHour() + " " + localDateTime.getMinute());
        System.out.println(zonedDateTime.getHour() + " " + zonedDateTime.getMinute() + zonedDateTime.getOffset());
        zonedDateTime = ZonedDateTime.now();
        System.out.println(zonedDateTime.getHour() + " " + zonedDateTime.getMinute() + zonedDateTime.getOffset());
    }
}
~~~

The output is:

17 35

17 35+11:00

17 35+11:00