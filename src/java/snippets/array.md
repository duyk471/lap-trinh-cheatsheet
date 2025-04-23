# Array
### AllEqualSnippet

```java
package array;

import java.util.Arrays;

/**
 * AllEqualSnippet.
 */
public class AllEqualSnippet {

  /**
   * Returns true if all elements in array are equal.
   *
   * @param arr the array to check (not null)
   * @param <T> the element type
   * @return true if all elements in the array are equal
   */
  public static <T> boolean allEqual(T[] arr) {
    return Arrays.stream(arr).distinct().count() == 1;
  }
}
```

### ArrayConcatSnippet

```java
package array;

import java.util.Arrays;

/**
 * ArrayConcatSnippet.
 */
public class ArrayConcatSnippet {

  /**
   * Generic 2 array concatenation Credits: Joachim Sauer https://stackoverflow.com/questions/80476/how-can-i-concatenate-two-arrays-in-java
   *
   * @param first  is the first array (not null)
   * @param second is the second array (not null)
   * @param <T>    the element type
   * @return concatenated array
   */
  public static <T> T[] arrayConcat(T[] first, T[] second) {
    var result = Arrays.copyOf(first, first.length + second.length);
    System.arraycopy(second, 0, result, first.length, second.length);
    return result;
  }
}
```

### 

```java
package array;

import java.util.Arrays;

/**
 * ArrayMeanSnippet.
 */
public class ArrayMeanSnippet {

  /**
   * Returns the mean of the integers in the array.
   *
   * @param arr the array of integers (not null)
   * @return a double representing the mean of the array
   */
  public static double arrayMean(int[] arr) {
    return (double) Arrays.stream(arr).sum() / arr.length;
  }
}
```

### ArrayMedianSnippet

```java
package array;

import java.util.Arrays;

/**
 * ArrayMedianSnippet.
 */
public class ArrayMedianSnippet {

  /**
   * Returns the median of the array.
   *
   * @param arr the array of integers (not null)
   * @return a double representing the median of the array
   */
  public static double arrayMedian(int[] arr) {
    Arrays.sort(arr);
    var mid = arr.length / 2;
    return arr.length % 2 != 0 ? (double) arr[mid] : (double) (arr[mid] + arr[mid - 1]) / 2;
  }
}
```

### ArrayModeSnippet

```java
package array;

import java.util.Arrays;

/**
 * ArrayModeSnippet.
 */
public class ArrayModeInPlaceSnippet {

  /**
  * Returns the mode of the array.
  *
  * @param arr array to find mode in it
  * @return mode of array
  */
  public static int modeArrayInPlace(int[] arr) {
    if (arr.length == 0) {
      return 0;
    }

    Arrays.sort(arr);

    int mode = arr[0];
    int maxcount = 1;
    int count = 1;

    for (int i = 1; i < arr.length; i++) {
      if (arr[i] == arr[i - 1]) {
        count++;
      } else {
        if (count > maxcount) {
          maxcount = count;
          mode = arr[i - 1];
        }
        count = 1;
      }
    }
    if (count > maxcount) {
      mode = arr[arr.length - 1];
    }
    return mode;
  }
}
```

### ArrayModeSnippet

```java
package array;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
/**
 * ArrayModeSnippet.
 */

public class ArrayModeSnippet {

  /**
   * Private constructor to prevent instantiation.
   */
  private ArrayModeSnippet() {
    throw new IllegalStateException("Utility class");
  }

  /**
   * Returns the mode(s) of the array.
   * If multiple modes exist, it returns them in a list.
   */
  public static List<Integer> modeArray(int[] arr) {
    int maxCount = 0;
    HashMap<Integer, Integer> frequencyMap = new HashMap<>();
    for (int num : arr) {
      frequencyMap.put(num, frequencyMap.getOrDefault(num, 0) + 1);
      maxCount = Math.max(maxCount, frequencyMap.get(num));
    }
    List<Integer> modes = new ArrayList<>();
    for (Map.Entry<Integer, Integer> entry : frequencyMap.entrySet()) {
      if (entry.getValue() == maxCount) {
        modes.add(entry.getKey());
      }
    }
    return modes;
  }
}
```

### ArraySumSnippet

```java
package array;

import java.util.Arrays;

/**
 * ArraySumSnippet.
 */
public class ArraySumSnippet {

  /**
   * Returns sum of the integers in the array.
   *
   * @param arr the array of integers (not null)
   * @return the sum of the elements from the array
   */
  public static int arraySum(int[] arr) {
    return Arrays.stream(arr).sum();
  }
}
```

### FindMaxSnippet

```java
package array;

import java.util.Arrays;

/**
 * FindMaxSnippet.
 */
public class FindMaxSnippet {

  /**
   * Returns the maximum integer from the array using reduction.
   *
   * @param arr the array of integers (not null)
   * @return the maximum element from the array
   */
  public static int findMax(int[] arr) {
    return Arrays.stream(arr).reduce(Integer.MIN_VALUE, Integer::max);
  }
}
```

### FindMinSnippet

```java
package array;

import java.util.Arrays;
 
/**
  * FindMinSnippet.
  */
public class FindMinSnippet {
 
  /**
    * Returns the minimum integer from the array using reduction.
    *
    * @param arr the array of integers (not null)
    * @return the minimum element from the array
    */
  public static int findMin(int[] arr) {
    return Arrays.stream(arr).reduce(Integer.MAX_VALUE, Integer::min);
  }
}
 
### MultiArrayConcatenationSnippet

```java
package array;

import java.util.Arrays;

/**
 * MultiArrayConcatenationSnippet.
 */
public class MultiArrayConcatenationSnippet {

  /**
   * Generic N array concatenation Credits: Joachim Sauer https://stackoverflow.com/questions/80476/how-can-i-concatenate-two-arrays-in-java
   *
   * @param first is the first array (not null)
   * @param rest  the rest of the arrays (optional)
   * @param <T>   the element type
   * @return concatenated array
   */
  public static <T> T[] multiArrayConcat(T[] first, T[]... rest) {
    var totalLength = first.length;
    for (var array : rest) {
      totalLength += array.length;
    }
    var result = Arrays.copyOf(first, totalLength);
    var offset = first.length;
    for (var array : rest) {
      System.arraycopy(array, 0, result, offset, array.length);
      offset += array.length;
    }
    return result;
  }
}
```

### ReverseArraySnippet

```java
package array;

/**
 * ReverseArraySnippet.
 */
public class ReverseArraySnippet {

  /**
   * The function then reverses the elements of the array between the starting and ending
   * indices using a while loop and a temporary variable `temp`. Finally, the function returns
   * the reversed array.
   *
   * @param array a array
   * @param start start index array
   * @param end end index array
   * @return reverses elements in the array
   * @throws IllegalArgumentException if the [start] index is greater
   *         than the [end] index or if the array is null
   **/
  public static <T> T[] reverseArray(T[] array, int start, int end) {
    if (start > end || array == null) {
      throw new
              IllegalArgumentException("Invalid argument!");
    }
    int minimumSizeArrayForReversal = 2;
    if (start == end || array.length < minimumSizeArrayForReversal) {
      return array;
    }
    while (start < end) {
      T temp = array[start];
      array[start] = array[end];
      array[end] = temp;
      start++;
      end--;
    }
    return array;
  }
}
```