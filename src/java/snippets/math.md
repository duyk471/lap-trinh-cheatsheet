
### DiceThrow

```java
package math;

import java.util.Random;

/**
 * Sum of Dice throw (Eg. 3d6 - 3 dice having 6 sides).
 */
public class DiceThrow {

  private static Random random = new Random();

  /**
  * Enum for standardized sided dice (4,6,8,10,12 and 20).
  */
  public enum DiceSides {

    FOUR(4), SIX(6), EIGHT(8), TEN(10), TWELVE(12), TWENTY(20);

    private final int diSides;

    DiceSides(int diceSides) {
      this.diSides = diceSides;
    }

    /**
     * Returns the number of sides of a dice.
     *
     * @return int denoting number of sides of a dice
     */
    public int getDiceSides() {
      return this.diSides;
    }

  }

  /**
  * Returns the sum of sides for the given number of sides of each dice.
  *
  * @param noOfDice number of dice
  * @param sides sides of a dice
  * @return int sum of sides for number of dice
  */
  public static int throwDice(int noOfDice, DiceSides sides) {

    int sum = 0;
    for (int i = 0; i < noOfDice; i++) {
      sum = sum + (1 + random.nextInt(sides.getDiceSides()));
    }
    return sum;
  }
}
```

### EloRatingSnippet

```java
package math;

/**
 * EloRatingSnippet.
 */
public class EloRatingSnippet {

  static final int BASE = 400; //Two types are popular - 400 and 480. We will choose 400 here
  static final int RATING_ADJUSTMENT_FACTOR = 32; //32 is the standard for Beginner Games

  /**
   * Elo Rating Snippet to calculate result after a single match.
   *
   * @param firstPlayerRating Rating of the first player.
   * @param secondPlayerRating Rating of the second player.
   * @param result Result of the match, always considered with respect to the first player.
   *               1 indicates a win, 0.5 indicates a draw and 0 indicates a loss.
   * @return Returns the new rating of the first player.
   */
  public static double calculateMatchRating(double firstPlayerRating, double secondPlayerRating,
      double result) {
    double ratingDiff = ((secondPlayerRating - firstPlayerRating) * 1.0) / BASE;
    double logisticDiff = Math.pow(10, ratingDiff);
    double firstPlayerExpectedScore = 1.0 / (1 + logisticDiff);
    double firstPlayerActualScore = result;
    double newRating = firstPlayerRating + RATING_ADJUSTMENT_FACTOR * (firstPlayerActualScore 
                       - firstPlayerExpectedScore);
    return newRating;
  }
}
```

### EvenOdd

```java
package math;

/**
 * EvenOdd.
 */
public class EvenOdd {

  /**
   * Returns string denoting number is odd or even.
   *
   * @param num To check whether its even or odd
   * @return string denoting its even or odd
   */
  public static String evenodd(int num) {
    if (num % 2 == 0) {
      return "even";
    } else {
      return "odd";
    }
  }
}
```

### FactorialSnippet

```java
package math;

/**
 * FactorialSnippet.
 */
public class FactorialSnippet {

  /**
   * Factorial. Works only for small numbers
   *
   * @param number for which factorial is to be calculated for
   * @return factorial
   */
  public static int factorial(int number) {
    var result = 1;
    for (var factor = 2; factor <= number; factor++) {
      result *= factor;
    }
    return result;
  }

  /**
   * Factorial. Example of what the recursive implementation looks like.
   *
   * @param number for which factorial is to be calculated for
   * @return factorial
   */
  public static int recursiveFactorial(int number) {
    var initial = 0;
    if (number == initial) {
      return initial + 1;
    }
    return number * recursiveFactorial(number - 1);
  }
}
```

### FibonacciSnippet

```java
package math;

import java.util.ArrayList;
import java.util.List;

/**
 * FibonacciSnippet.
 */
public class FibonacciSnippet {

  /**
   * Recursive Fibonacci series. Works only for small n and is spectacularly inefficient
   *
   * @param n given number
   * @return fibonacci number for given n
   */
  public static int fibonacci(int n) {
    if (n <= 1) {
      return n;
    } else {
      return fibonacci(n - 1) + fibonacci(n - 2);
    }
  }

  /**
   * Fibonacci series using dynamic programming. Works for larger ns as well.
   *
   * @param n given number
   * @return fibonacci number for given n
   */
  public static int fibonacciBig(int n) {
    int previous = 0;
    int current = 1;
    for (int i = 0; i < n - 1; i++) {
      int t = previous + current;
      previous = current;
      current = t;
    }

    return current;
  }

  /**
   * Example of what an iterative implementation of Fibonacci looks like.
   *
   * @param number given number
   * @return fibonacci number for given n
   */
  public static int iterativeFibonacci(int number) {
    List<Integer> list = new ArrayList<>();
    list.add(0);
    list.add(1);
    for (int i = 2; i < number + 1; i++) {
      list.add(list.get(i - 2) + list.get(i - 1));
    }
    return list.get(number);
  }
}
```

### GreatestCommonDivisorSnippet

```java
package math;

/**
 * GreatestCommonDivisorSnippet.
 */
public class GreatestCommonDivisorSnippet {

  /**
   * Greatest common divisor calculation.
   *
   * @param a one of the numbers whose gcd is to be computed
   * @param b other number whose gcd is to be computed
   * @return gcd of the two numbers
   */
  public static int gcd(int a, int b) {
    if (b == 0) {
      return a;
    }
    return gcd(b, a % b);
  }
}
```

### HaversineFormulaSnippet

```java
package math;

/**
 * HaversineFormulaSnippet.
 */
public class HaversineFormulaSnippet {

  // Radius of sphere on which the points are, in this case Earth.
  private static final double SPHERE_RADIUS_IN_KM = 6372.8;

  /**
   * Haversine formula for calculating distance between two latitude, longitude points.
   *
   * @param latA Latitude of point A
   * @param longA Longitude of point A
   * @param latB Latitude of point B
   * @param longB Longitude of point B
   * @return the distance between the two points.
   */
  public static double findHaversineDistance(double latA, double longA, double latB, double longB) {
    if (!isValidLatitude(latA)
        || !isValidLatitude(latB)
        || !isValidLongitude(longA)
        || !isValidLongitude(longB)) {
      throw new IllegalArgumentException();
    }

    // Calculate the latitude and longitude differences
    var latitudeDiff = Math.toRadians(latB - latA);
    var longitudeDiff = Math.toRadians(longB - longA);

    var latitudeA = Math.toRadians(latA);
    var latitudeB = Math.toRadians(latB);

    // Calculating the distance as per haversine formula
    var a = Math.pow(Math.sin(latitudeDiff / 2), 2)
            + Math.pow(Math.sin(longitudeDiff / 2), 2) * Math.cos(latitudeA) * Math.cos(latitudeB);
    var c = 2 * Math.asin(Math.sqrt(a));
    return SPHERE_RADIUS_IN_KM * c;
  }

  // Check for valid latitude value
  private static boolean isValidLatitude(double latitude) {
    return latitude >= -90 && latitude <= 90;
  }

  // Check for valid longitude value
  private static boolean isValidLongitude(double longitude) {
    return longitude >= -180 && longitude <= 180;
  }
}
```

### LeastCommonMultipleSnippet

```java
package math;

/**
 * LeastCommonMultipleSnippet.
 */
public class LeastCommonMultipleSnippet {
  /**
   * Least common multiple  calculation.
   *
   * @param a one of the numbers whose lcm is to be computed
   * @param b other number whose lcm is to be computed
   * @return lcm of the two numbers
   */
  public static int lcm(int a, int b) {
    int max = a > b ? a : b;
    int min = a < b ? a : b;
    for (int i = 1; i <= min; i += 1) {
      int prod = max * i;
      if (prod % min == 0) {
        return prod;
      }
    }
    return max * min;
  }
}
```

### LuhnSnippet

```java
package math;

/**
 * LuhnSnippet.
 */
public class LuhnSnippet {

  /**
   * Calculates checksum for a given number with Luhn's algorithm. Works only on non-negative
   * integers not greater than {@link Long#MAX_VALUE} i.e., all numbers with a maximum of 18
   * digits, plus 19-digit-long numbers start with 1..8 (also some with 9, too). For
   * demonstration purposes, algorithm is not optimized for efficiency.
   *
   * @param num number whose checksum is to be calculated
   * @return checksum value for num
   * @see <a href="https://patents.google.com/patent/US2950048A">Hans P. LUHN's patent US2950048A</a>
   * @see <a href="https://en.wikipedia.org/wiki/Luhn_algorithm">Luhn algorithm on Wikipedia</a>
   */
  public static int calculateLuhnChecksum(long num) {
    if (num < 0) {
      throw new IllegalArgumentException("Non-negative numbers only.");
    }
    final var numStr = String.valueOf(num);

    var sum = 0;
    var isOddPosition = true;
    // Loop on digits of numStr from right to left.
    for (var i = numStr.length() - 1; i >= 0; i--) {
      final var digit = Integer.parseInt(Character.toString(numStr.charAt(i)));
      final var substituteDigit = (isOddPosition ? 2 : 1) * digit;

      final var tensPlaceDigit = substituteDigit / 10;
      final var onesPlaceDigit = substituteDigit % 10;
      sum += tensPlaceDigit + onesPlaceDigit;

      isOddPosition = !isOddPosition;
    }
    final var checksumDigit = (10 - (sum % 10)) % 10;
    // Outermost modulus handles edge case `num = 0`.
    return checksumDigit;
  }
}
```

### NaturalNumberBinaryConversionSnippet

```java
package math;

import java.util.Stack;
import java.util.stream.Collectors;
import java.util.stream.IntStream;
import java.util.stream.Stream;

/**
 * NaturalNumberBinaryConversionSnippet.
 */
public class NaturalNumberBinaryConversionSnippet {

  /**
   * Convert natural number to binary string. Only supports positive integers.Throws exception
   * for negative integers
   *
   * @param naturalNumber given number
   * @return Binary string representation of naturalNumber
   */
  public static String toBinary(long naturalNumber) {
    if (naturalNumber < 0) {
      throw new NumberFormatException("Negative Integer, this snippet only accepts "
              + "positive integers");
    }
    if (naturalNumber == 0) {
      return "0";
    }
    final Stack<Long> binaryBits =
            Stream.iterate(naturalNumber, n -> n > 0, n -> n / 2).map(n -> n % 2)
                    .collect(Stack::new, Stack::push, Stack::addAll);
    return Stream.generate(binaryBits::pop)
            .limit(binaryBits.size()).map(String::valueOf).collect(Collectors.joining());
  }

  /**
   * Convert binary string representation to Long valued Integer. Throws exception if input
   * string contains characters other than '0' and '1'
   *
   * @param binary given number
   * @return Unsigned Long value for the binary number
   */
  public static Long fromBinary(String binary) {
    binary.chars().filter(c -> c != '0' && c != '1').findFirst().ifPresent(in -> {
      throw new NumberFormatException(
              "Binary string contains values other than '0' and '1'");
    });
    return IntStream.range(0, binary.length())
            .filter(in -> binary.charAt(binary.length() - 1 - in) == '1')
            .mapToLong(in -> ((long) 0b1) << in).sum();
  }
}
```

### PerformLotterySnippet

```java
package math;

import java.util.ArrayList;
import java.util.Collections;

/**
 * PerformLotterySnippet.
 */
public class PerformLotterySnippet {

  /**
   * Generate random lottery numbers.
   *
   * @param numNumbers    how many performLottery numbers are available (e.g. 49)
   * @param numbersToPick how many numbers the player needs to pick (e.g. 6)
   * @return array with the random numbers
   */
  public static Integer[] performLottery(int numNumbers, int numbersToPick) {
    var numbers = new ArrayList<Integer>();
    for (var i = 0; i < numNumbers; i++) {
      numbers.add(i + 1);
    }
    Collections.shuffle(numbers);
    return numbers.subList(0, numbersToPick).toArray(new Integer[numbersToPick]);
  }
}
```

### PrimeNumberSnippet

```java
package math;

/**
 * PrimeNumberSnippet.
 */
public class PrimeNumberSnippet {

  /**
   * Checks if given number is a prime number. Prime number is a number that is greater than 1 and
   * divided by 1 or itself only Credits: https://en.wikipedia.org/wiki/Prime_number
   *
   * @param number number to check prime
   * @return true if prime
   */
  public static boolean isPrime(int number) {
    //if number < 2 its not a prime number
    if (number < 2) {
      return false;
    }
    // 2 and 3 are prime numbers
    if (number < 3) {
      return true;
    }
    // check if n is a multiple of 2
    if (number % 2 == 0) {
      return false;
    }
    // if not, then just check the odds
    for (var i = 3; i * i <= number; i += 2) {
      if (number % i == 0) {
        return false;
      }
    }
    return true;
  }
}
```

### RandomNumber

```java
package math;

import java.util.Random;

/**
 * Random Number between given two values.
 * Supported Data types - Byte, Short, Integer, Long, Float and Double.
 */
public class RandomNumber {

  private RandomNumber() {}

  private static Random random = new Random();

  /**
  * Return a random number between two given numbers.
  *
  * @param start Starting point to find the random number
  * @param end Ending point to find the random number
  * @return Number denoting the random number generated
  */
  public static <T extends Number> Number getRandomNumber(T start, T end) {

    if (start instanceof Byte && end instanceof Byte) {
      return (byte) (start.byteValue()
              + random.nextInt(end.byteValue() - start.byteValue() + 1));
    } else if (start instanceof Short && end instanceof Short) {
      return (short) (start.shortValue()
              + random.nextInt(end.shortValue() - start.shortValue() + 1));
    } else if (start instanceof Integer && end instanceof Integer) {
      return start.intValue()
              + random.nextInt(end.intValue() - start.intValue() + 1);
    } else if (start instanceof Long && end instanceof Long) {
      return start.longValue()
              + (long) (random.nextDouble() * end.longValue() - start.longValue() + 1);
    } else if (start instanceof Float && end instanceof Float) {
      return start.floatValue()
              + random.nextFloat() * (end.floatValue() - start.floatValue());
    } else if (start instanceof Double && end instanceof Double) {
      return start.doubleValue()
              + random.nextDouble() * (end.doubleValue() - start.doubleValue());
    } else {
      throw new IllegalArgumentException("Invalid Numbers As Arguments "
              + start.getClass() + " and " + end.getClass());
    }
  }
}


### SquareRoot

```java
package math;

/**
 * SquareRoot.
 */
public class SquareRoot {

  /**
   * Returns square root of a number.
   *
   * @param num To find SquareRoot
   * @param p   precision till how many decimal numbers we want accurate ans
   */
  public static double sqrt(int num, int p) {
    int start = 0;
    int end = num;
    double root = 0.0;

    while (start <= end) {
      int mid = start + (end - start) / 2;

      if ((mid * mid) > num) {
        end = mid - 1;
      } else if ((mid * mid) < num) {
        start = mid + 1;
      } else {
        return mid;
      }
    }
    double incr = 0.1;
    for (int i = 0; i < p; i++) {
      while (root * root < num) {
        root = root + incr;
      }
      root = root - incr;
      incr = incr / 10;
    }
    return root;
  }
}
```
