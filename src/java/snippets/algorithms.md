### BinarySearchIn2dArraySnippet

```java
package algorithm;

/**
 * BinarySearchIn2dArraySnippet.
 */
public class BinarySearchIn2dArraySnippet {

  /**
  * Search an item with binarySearch algorithm.
  *
  * @param matrix should be sorted
  * @param target an item to search
  * @return if location of item is found, otherwise return {-1,-1}
  */
  public static int[] binarySearchIn2darr(int[][] matrix, int target) {
    int rows = matrix.length - 1;
    int cols = matrix[0].length - 1;

    if (rows == 1) {
      return binarySearch(matrix, target, 0, 0, cols);
    }

    int rstart = 0;
    int rend = rows;
    int cmid = cols / 2;

    while (rstart < rend - 1) {
      int rmid = rstart + (rend - rstart) / 2;
      if (matrix[rmid][cmid] > target) {
        rend = rmid;
      } else if (matrix[rmid][cmid] < target) {
        rstart = rmid;
      } else {
        return new int[]{rmid, cmid};
      }
    }
    if (matrix[rstart][cmid] == target) {
      return new int[]{rstart, cmid};
    }
    if (matrix[rend][cmid] == target) {
      return new int[]{rend, cmid};
    }
    if (target <= matrix[rstart][cmid - 1]) {
      return binarySearch(matrix, target, rstart, 0, cmid - 1);
    }
    if (target >= matrix[rstart][cmid + 1]) {
      return binarySearch(matrix, target, rstart, cmid + 1, cols);
    }
    if (target <= matrix[rend][cmid - 1]) {
      return binarySearch(matrix, target, rend, 0, cmid - 1);
    }
    if (target <= matrix[rend][cmid + 1]) {
      return binarySearch(matrix, target, rend, cmid + 1, cols);
    }
    return new int[]{-1, -1};
  }

  static int[] binarySearch(int[][] matrix, int target, int row, int cstart, int cend) {
    while (cstart <= cend) {
      int cmid = cstart + (cend - cstart) / 2;
      if (matrix[row][cmid] > target) {
        cend = cmid - 1;
      } else if (matrix[row][cmid] < target) {
        cstart = cend + 1;
      } else {
        return new int[]{row, cmid};
      }
    }
    return new int[]{-1, -1};
  }
}
```

### BinarySearchSnippet

```java
package algorithm;

/**
 * BinarySearchSnippet.
 */
public class BinarySearchSnippet {

  /**
   * Search an item with binarySearch algorithm.
   *
   * @param arr sorted array to search
   * @param item an item to search
   * @return if item is found, return the index position of the array item otherwise return -1
   */

  public static int binarySearch(int[] arr, int left, int right, int item) {
    if (right >= left) {
      int mid = left + (right - left) / 2;
      if (arr[mid] == item) {
        return mid;
      }

      if (arr[mid] > item) {
        return binarySearch(arr, left, mid - 1, item);
      }

      return binarySearch(arr, mid + 1, right, item);
    }
    return -1;
  }
}
```

### BubbleSortSnippet

```java
package algorithm;

/**
 * BubbleSortSnippet.
 */
public class BubbleSortSnippet {

  /**
   * Sort an array with bubbleSort algorithm.
   *
   * @param arr array to sort
   */
  public static void bubbleSort(int[] arr) {
    var lastIndex = arr.length - 1;

    for (var j = 0; j < lastIndex; j++) {
      for (var i = 0; i < lastIndex - j; i++) {
        if (arr[i] > arr[i + 1]) {
          var tmp = arr[i];
          arr[i] = arr[i + 1];
          arr[i + 1] = tmp;
        }
      }
    }
  }
}
```

### CountingSortSnippet

```java
package algorithm;

import java.util.Arrays;

/**
 * CountingSortSnippet.
 */
public class CountingSortSnippet {

  /**
   * Sort an array having zero or positive numbers with countingSort algorithm.
   *
   * @param arr array to sort
   */
  public static void countingSort(int[] arr) {
    var max = Arrays.stream(arr).max().getAsInt();

    var count = new int[max + 1];

    for (var num : arr) {
      count[num]++;
    }

    for (var i = 1; i <= max; i++) {
      count[i] += count[i - 1];
    }

    var sorted = new int[arr.length];
    for (var i = arr.length - 1; i >= 0; i--) {
      var cur = arr[i];
      sorted[count[cur] - 1] = cur;
      count[cur]--;
    }

    var index = 0;
    for (var num : sorted) {
      arr[index++] = num;
    }
  }
}
```

### CycleSortSnippet

```java
package algorithm;

/**
 * CycleSortSnippet.
 */
public class CycleSortSnippet {

  /**
   * Sort an array with cycleSort algorithm.
   *
   * @param arr array to sort
   */
  public static int[] cycleSort(int[] arr) {
    int n = arr.length;
    int i = 0;
    while (i < n) {
      int correctpos = arr[i] - 1;
      if (arr[i] != arr[correctpos]) {
        int temp = arr[i];
        arr[i] = arr[correctpos];
        arr[correctpos] = temp;
      } else {
        i++;
      }
    }
    return arr;
  }
}
```

### DammSnippet

```java
package algorithm;

/**
 * The implementation of the Damm algorithm based on the details on
 * <a href="https://en.wikipedia.org/wiki/Damm_algorithm">Wikipedia</a>.
 *
 * <p>The Damm algorithm is used for error detection and generates a checksum
 * that can detect single-digit errors and adjacent transposition errors.</p>
 *
 */
public class DammSnippet {

  /**
   * Private constructor to prevent instantiation of utility class.
   */
  private DammSnippet() {
    throw new UnsupportedOperationException("Utility class - instantiation is not allowed.");
  }

  /**
   * The quasigroup table used by the Damm algorithm.
   */
  private static final int[][] matrix = new int[][] {
          { 0, 3, 1, 7, 5, 9, 8, 6, 4, 2 },
          { 7, 0, 9, 2, 1, 5, 4, 8, 6, 3 },
          { 4, 2, 0, 6, 8, 7, 1, 3, 5, 9 },
          { 1, 7, 5, 0, 9, 8, 3, 4, 2, 6 },
          { 6, 1, 2, 3, 0, 4, 5, 9, 7, 8 },
          { 3, 6, 7, 4, 2, 0, 9, 5, 8, 1 },
          { 5, 8, 6, 9, 7, 2, 0, 1, 3, 4 },
          { 8, 9, 4, 5, 3, 6, 2, 0, 1, 7 },
          { 9, 4, 3, 8, 6, 1, 7, 2, 0, 5 },
          { 2, 5, 8, 1, 4, 3, 6, 7, 9, 0 }
  };

  /**
   * Calculates the Damm checksum digit for the given number.
   *
   * @param number the input number as a string
   * @return the calculated checksum digit
   * @throws IllegalArgumentException if the input is null, empty, or contains non-digit characters
   */
  public static int calculateCheckSumDigit(String number) {
    if (number == null || number.isEmpty()) {
      throw new IllegalArgumentException("Input number cannot be null or empty.");
    }

    int interim = 0;
    for (int index = 0; index < number.length(); index++) {
      char currCh = number.charAt(index);
      if (!Character.isDigit(currCh)) {
        throw new IllegalArgumentException("Input number contains invalid characters: " + number);
      }

      int currentIndex = currCh - '0';
      interim = matrix[interim][currentIndex];
    }

    return interim;
  }

  /**
   * Calculates the Damm checksum digit for the given number.
   *
   * @param number the input number as an integer
   * @return the calculated checksum digit
   */
  public static int calculateCheckSumDigit(int number) {
    return calculateCheckSumDigit(String.valueOf(number));
  }

  /**
   * Calculates the Damm checksum digit for the given number.
   *
   * @param number the input number as a long
   * @return the calculated checksum digit
   */
  public static int calculateCheckSumDigit(long number) {
    return calculateCheckSumDigit(String.valueOf(number));
  }

  /**
   * Appends the calculated checksum digit to the given number as a string.
   *
   * @param number the input number as a string
   * @return the original number with the checksum digit appended
   * @throws IllegalArgumentException if the input is null, empty, or contains non-digit characters
   */
  public static String generateCheckSum(String number) {
    int checkSumDigit = calculateCheckSumDigit(number);
    return number + checkSumDigit;
  }

  /**
   * Appends the calculated checksum digit to the given number as an integer.
   *
   * @param number the input number as an integer
   * @return the original number with the checksum digit appended
   */
  public static int generateCheckSum(int number) {
    int checkSumDigit = calculateCheckSumDigit(number);
    return (number * 10) + checkSumDigit;
  }

  /**
   * Appends the calculated checksum digit to the given number as a long.
   *
   * @param number the input number as a long
   * @return the original number with the checksum digit appended
   */
  public static long generateCheckSum(long number) {
    int checkSumNumber = calculateCheckSumDigit(number);
    return (number * 10) + checkSumNumber;
  }

  /**
   * Validates the given number by checking if the checksum digit is correct.
   *
   * @param number the input number as a string
   * @return {@code true} if the number is valid, {@code false} otherwise
   * @throws IllegalArgumentException if the input is null, empty, or contains non-digit characters
   */
  public static boolean validate(String number) {
    return calculateCheckSumDigit(number) == 0;
  }

  /**
   * Validates the given number by checking if the checksum digit is correct.
   *
   * @param number the input number as an integer
   * @return {@code true} if the number is valid, {@code false} otherwise
   */
  public static boolean validate(int number) {
    return calculateCheckSumDigit(number) == 0;
  }

  /**
   * Validates the given number by checking if the checksum digit is correct.
   *
   * @param number the input number as a long
   * @return {@code true} if the number is valid, {@code false} otherwise
   */
  public static boolean validate(long number) {
    return calculateCheckSumDigit(number) == 0;
  }
}
```

### InsertionSortSnippet

```java
package algorithm;

/**
 * InsertionSortSnippet.
 */
public class InsertionSortSnippet {

  /**
   * Sort an array with insertionSort algorithm.
   *
   * @param arr array to sort
   */
  public static void insertionSort(int[] arr) {
    for (var i = 1; i < arr.length; i++) {
      var tmp = arr[i];
      var j = i - 1;

      while (j >= 0 && arr[j] > tmp) {
        arr[j + 1] = arr[j];
        j--;
      }
      arr[j + 1] = tmp;
    }
  }
}
```

### LinearSearchIn2dArraySnippet

```java
package algorithm;

/**
 * LinearSearchIn2dArraySnippet.
 */
public class LinearSearchIn2dArraySnippet {

  /**
   * Search an item with linearSearch algorithm.
   *
   * @param arr    array to search
   * @param target an item to search
   * @return if location of target is found,otherwise return {-1,-1}
   */

  public static int[] linearSearch2dArray(int[][] arr, int target) {

    for (int i = 0; i < arr.length; i++) {
      for (int j = 0; j < arr[i].length; j++) {
        if (arr[i][j] == target) {
          return new int[]{i, j};
        }
      }
    }
    return new int[]{-1, -1};
  }
}
```

### LinearSearchSnippet

```java
package algorithm;

/**
 * LinearSearchSnippet.
 */
public class LinearSearchSnippet {

  /**
   * Search an item with linearSearch algorithm.
   *
   * @param arr array to search
   * @param item an item to search
   * @return if item is found, return the index position of the array item otherwise return -1
   */
  public static int linearSearch(int[] arr, int item) {
    for (int i = 0; i < arr.length; i++) {
      if (item == arr[i]) {
        return i;
      }
    }
    return -1;
  }
}
```

### LuhnModnSnippet

```java
package algorithm;

/**
 * LuhnModnSnippet.
 */
public class LuhnModnSnippet {

  private static final String CODE_POINTS = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ";

  /**
   * Generates a check character using the Luhn mod N algorithm.
   *
   * @param character the input string consisting of valid alphanumeric characters
   * @return the generated check character
   * @throws IllegalArgumentException if the input contains invalid characters
   */
  public static int codePointFromCharacter(char character) {
    if (CODE_POINTS.indexOf(character) == -1) {
      throw new IllegalArgumentException("Invalid character: " + character);
    }
    return CODE_POINTS.indexOf(character);
  }

  /**
   * Converts a code point to its corresponding character.
   *
   * @param codePoint the code point to be converted
   * @return the character representation of the code point
   * @throws IllegalArgumentException if the code point is out of range.
   */
  public static char characterFromCodePoint(int codePoint) {
    if (codePoint < 0 || codePoint >= CODE_POINTS.length()) {
      throw new IllegalArgumentException("Invalid code point: " + codePoint);
    }
    return CODE_POINTS.charAt(codePoint);
  }

  public static int numberOfValidInputCharacters() {
    return CODE_POINTS.length();
  }

  /**
   * Helper method to calculate the sum for both check character generation and validation.
   *
   * @param input the input string
   * @param factorStart the initial factor to start with (1 or 2)
   * @return the calculated sum, reminder, and the numberOfValidInputCharacters
   */
  private static int[] calculateSum(String input, int factorStart) {
    if (input == null || input.isEmpty()) {
      throw new IllegalArgumentException("Input cannot be empty");
    }

    int factor = factorStart;
    int sum = 0;
    int n = numberOfValidInputCharacters();

    for (int i = input.length() - 1; i >= 0; i--) {
      int codePoint = codePointFromCharacter(input.charAt(i));
      int addend = factor * codePoint;
      factor = (factor == 2) ? 1 : 2;
      addend = (addend / n) + (addend % n);
      sum += addend;
    }
    return new int[]{sum, sum % n, n};
  }

  /**
   * Generates a check character for the given input string using the Luhn mod N algorithm.
   *
   * @param input the input string (non-empty)
   * @return the generated check character
   * @throws IllegalArgumentException if the input is null or empty
   */
  public static char generateCheckCharacter(String input) {
    int[] result = calculateSum(input, 2);
    return characterFromCodePoint((result[2] - result[1]) % result[2]);
  }

  /**
   * Validates a check character by applying the Luhn mod N algorithm.
   *
   * @param input the input string (including the check character)
   * @return true if the input passes validation, false otherwise
   * @throws IllegalArgumentException if the input is null or empty
   */
  public static boolean validateCheckCharacter(String input) {
    int[] result = calculateSum(input, 1);
    return (result[1] == 0);
  }
}
```

### MergeSortSnippet

```java
package algorithm;

/**
 * MergeSortSnippet.
 */
public class MergeSortSnippet {  
  /**
     * Sort an array with qmergesort algorithm.
     *
     * @param arr   array to sort
     * @low low index where to begin sort (e.g. 0)
     * @high high index where to end sort (e.g. array length - 1)
     */

  public static void mergeSort(int[] arr, int low, int high) {
    if (low >= high) {
      return;
    }
    var mid = (low + high) / 2;
    mergeSort(arr, low, mid);
    mergeSort(arr, mid + 1, high);
    merge(arr, low, high, mid);
  }

  private static void merge(int[] arr, int low, int high, int mid) {
    int[] temp = new int[(high - low + 1)];
    var i = low;
    var j = mid + 1;
    var k = 0;

    while (i <= mid && j <= high) {
      if (arr[i] < arr[j]) {
        temp[k++] = arr[i];
        i++;
      } else {
        temp[k++] = arr[j];
        j++;
      }
    }

    while (i <= mid) {
      temp[k++] = arr[i];
      i++;
    }

    while (j <= high) {
      temp[k++] = arr[j];
      j++;
    }

    for (int m = 0, n = low; m < temp.length; m++, n++) {
      arr[n] = temp[m];
    }
  }
}
```

### QuickSortSnippet

```java
package algorithm;

/**
 * QuickSortSnippet.
 */
public class QuickSortSnippet {

  /**
   * Sort an array with quicksort algorithm.
   *
   * @param arr   array to sort
   * @param left  left index where to begin sort (e.g. 0)
   * @param right right index where to end sort (e.g. array length - 1)
   */
  public static void quickSort(int[] arr, int left, int right) {
    var pivotIndex = left + (right - left) / 2;
    var pivotValue = arr[pivotIndex];
    var i = left;
    var j = right;
    while (i <= j) {
      while (arr[i] < pivotValue) {
        i++;
      }
      while (arr[j] > pivotValue) {
        j--;
      }
      if (i <= j) {
        var tmp = arr[i];
        arr[i] = arr[j];
        arr[j] = tmp;
        i++;
        j--;
      }
      if (left < i) {
        quickSort(arr, left, j);
      }
      if (right > i) {
        quickSort(arr, i, right);
      }
    }
  }
}
```

### SelectionSortSnippet

```java
package algorithm;

/**
 * SelectionSortSnippet.
 */
public class SelectionSortSnippet {

  /**
   * Sort an array with selectionSort algorithm.
   *
   * @param arr array to sort
   */
  public static void selectionSort(int[] arr) {
    var len = arr.length;

    for (var i = 0; i < len - 1; i++) {
      var minIndex = i;

      for (var j = i + 1; j < len; j++) {
        if (arr[j] < arr[minIndex]) {
          minIndex = j;
        }
      }

      var tmp = arr[minIndex];
      arr[minIndex] = arr[i];
      arr[i] = tmp;
    }
  }
}
### SieveOfEratosthenesSnippet

```java
package algorithm;

/**
 * SieveOfEratosthenesSnippet.
 */
public class SieveOfEratosthenesSnippet {
  /**
   * Search an item with binarySearch algorithm.
   *
   * @param n range of number.
   * @return isPrime boolean array where prime number 0 to n are mark true.
   */
  public static boolean[] sieveOfEratosthenes(int n) {
    boolean[] isPrime = new boolean[n + 1];
    for (int i = 0; i < isPrime.length; i++) {
      isPrime[i] = true;
    }

    for (int i = 2; i * i <= n; i++) {
      if (isPrime[i] == true) {
        for (int j = i * i; j <= n; j += i) {
          isPrime[j] = false;
        }
      }
    }

    return isPrime;
  }
}
```

### VerhoeffSnippet

```java
package algorithm;

/**
 * VerhoeffSnippet.
 */
public class VerhoeffSnippet {

  private static final int[][] d = {
          {0, 1, 2, 3, 4, 5, 6, 7, 8, 9},
          {1, 0, 3, 2, 5, 4, 7, 6, 9, 8},
          {2, 3, 0, 1, 6, 7, 4, 5, 8, 9},
          {3, 2, 1, 0, 7, 6, 5, 4, 9, 8},
          {4, 5, 6, 7, 0, 1, 2, 3, 8, 9},
          {5, 4, 7, 6, 1, 0, 3, 2, 9, 8},
          {6, 7, 4, 5, 2, 3, 0, 1, 8, 9},
          {7, 6, 5, 4, 3, 2, 1, 0, 9, 8},
          {8, 9, 8, 9, 8, 9, 8, 9, 0, 1},
          {9, 8, 9, 8, 9, 8, 9, 8, 1, 0}
  };

  private static final int[][] p = {
          {0, 1, 2, 3, 4, 5, 6, 7, 8, 9},
          {1, 5, 7, 6, 2, 8, 3, 0, 9, 4},
          {5, 8, 0, 3, 7, 9, 6, 1, 4, 2},
          {8, 9, 1, 6, 0, 4, 3, 5, 2, 7},
          {9, 4, 5, 3, 1, 2, 6, 8, 7, 0},
          {4, 2, 8, 6, 5, 7, 3, 9, 0, 1},
          {2, 7, 9, 3, 8, 0, 6, 4, 1, 5},
          {7, 0, 4, 6, 9, 1, 3, 2, 5, 8}
  };

  private static final int[] inv = {0, 4, 3, 2, 1, 5, 6, 7, 8, 9};

  /**
   * Validates a number using the Verhoeff checksum algorithm.
   *
   * @param num the numeric string to validate
   * @return true if the number is valid according to Verhoeff algorithm, false otherwise
   */
  public static boolean validateVerhoeff(String num) {
    int c = 0;
    int length = num.length();

    // Adjust index for validation of the full number (including check digit)
    for (int i = 0; i < length; i++) {
      int digit = Character.getNumericValue(num.charAt(length - i - 1));
      c = d[c][p[(i + 1) % 8][digit]]; // Correct permutation index
    }

    return c == 0; // Final checksum must be zero
  }

  /**
   * Generates a Verhoeff check digit for a given numeric string.
   *
   * @param num the numeric string for which to generate the check digit
   * @return the generated Verhoeff check digit as a string
   */
  public static String generateVerhoeff(String num) {
    int c = 0;
    int length = num.length();

    for (int i = 0; i < length; i++) {
      int digit = Character.getNumericValue(num.charAt(length - i - 1));
      c = d[c][p[(i % 8)][digit]];
    }

    return Integer.toString(inv[c]);
  }

}

```