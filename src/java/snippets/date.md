# Date
### AddDaysToDateSnippet

```java
package date;

import java.time.LocalDate;
import java.time.ZoneId;
import java.util.Calendar;
import java.util.Date;

/**
 * AddDaysToDateSnippet.
 */
public class AddDaysToDateSnippet {

  /**
   * Add days to given date.
   *
   * @param date given date
   * @param noOfDays number of days to add
   * @return modified date
   */
  public static Date addDaysToDate(Date date, int noOfDays) {
    if (date != null) {
      Calendar cal = Calendar.getInstance();
      cal.setTime(date);
      cal.add(Calendar.DAY_OF_MONTH, noOfDays);
      return cal.getTime();
    }
    return null;
  }

  /**
   * Add days to local date.
   *
   * @param date given local date
   * @param noOfDays number of days to add
   * @return modified date
   */
  public static LocalDate addDaysToLocalDate(LocalDate date, long noOfDays) {
    return date != null ? date.plusDays(noOfDays) : null;
  }
}
```

### DateDifferenceSnippet

```java
package date;

import java.time.LocalDate;
import java.time.temporal.ChronoUnit;

/**
 * DateDifferenceSnippet.
 */

public class DateDifferenceSnippet {

  /**
  * This function calculates the number of years between two LocalDate objects.
  * If the result is negative, it returns the absolute value of the difference.
  *
  * @param firstTime  The first LocalDate object representing the starting date
  * @param secondTime The second LocalDate object representing the ending date
  * @return The number of years between the two LocalDate objects as a long data type
  */
  public static long getYearsDifference(LocalDate firstTime, LocalDate secondTime) {
    var yearsDifference = ChronoUnit.YEARS.between(firstTime, secondTime);
    return Math.abs(yearsDifference);
  }

  /**
   * This function calculates the number of months between two LocalDate objects.
   * If the result is negative, it returns the absolute value of the difference.
   *
   * @param firstTime  The first LocalDate object representing the starting date
   * @param secondTime The second LocalDate object representing the ending date
   * @return The number of months between the two LocalDate objects as a long data type
   */
  public static long getMonthsDifference(LocalDate firstTime, LocalDate secondTime) {
    var monthsDifference = ChronoUnit.MONTHS.between(firstTime, secondTime);
    return Math.abs(monthsDifference);
  }

  /**
   * This function calculates the number of days between two LocalDate objects.
   * If the result is negative, it returns the absolute value of the difference.
   *
   * @param firstTime  The first LocalDate object representing the starting date
   * @param secondTime The second LocalDate object representing the ending date
   * @return The number of days between the two LocalDate objects as a long data type
   */
  public static long getDaysDifference(LocalDate firstTime, LocalDate secondTime) {
    var daysDifference = ChronoUnit.DAYS.between(firstTime, secondTime);
    return Math.abs(daysDifference);
  }
}

```