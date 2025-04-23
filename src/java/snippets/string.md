
### AnagramSnippet

```java
package string;

import java.util.Arrays;

/**
 * AnagramSnippet.
 */
public class AnagramSnippet {

  /**
   * Checks if two words are anagrams (contains same characters with same frequency in any order).
   *
   * @param s1 The first string to be checked
   * @param s2 The second string to be checked
   * @return true if they are anagrams of each other
   */
  public static boolean isAnagram(String s1, String s2) {
    var l1 = s1.length();
    var l2 = s2.length();

    if (l1 != l2) {
      return false;
    }

    var arr1 = new int[256];
    var arr2 = new int[256];
    
    for (var i = 0; i < l1; i++) {
      arr1[s1.charAt(i)]++;
      arr2[s2.charAt(i)]++;
    }
    return Arrays.equals(arr1, arr2);
  }
}
```

### CommonLettersSnippet

```java
package string;

import java.util.HashSet;
import java.util.Set;

/**
 * CommonLettersSnippet.
 */
public class CommonLettersSnippet {

  /**
   * Find Common Characters inside given two strings.
   *
   * @param firstStr  first string
   * @param secondStr second string
   * @return Common Characters.
   */
  public static String getCommonLetters(String firstStr, String secondStr) {
    Set<String> commonLetters = new HashSet<>();
    for (Character currentCharacter : firstStr.toCharArray()) {
      if (isCommonLetter(secondStr, currentCharacter)) {
        commonLetters.add(currentCharacter.toString());
      }
    }
    return String.join(" ", commonLetters);
  }

  private static boolean isCommonLetter(String str, Character character) {
    return str.contains(character.toString()) && Character.isLetter(character);
  }
}
```

### CompareVersionSnippet

```java
package string;

/**
 * CompareVersionSnippet.
 */
public class CompareVersionSnippet {

  private static final String EXTRACT_VERSION_REGEX = ".*?((?<!\\w)\\d+([.-]\\d+)*).*";

  /**
   * Compares two version strings.
   * Credits: https://stackoverflow.com/a/6702000/6645088 and https://stackoverflow.com/a/44592696/6645088
   *
   * @param v1 the first version string to compare
   * @param v2 the second version string to compare
   * @return the value {@code 0} if the two strings represent same versions;
   *     a value less than {@code 0} if {@code v1} is greater than {@code v2}; and
   *     a value greater than {@code 0} if {@code v2} is greater than {@code v1}
   */
  public static int compareVersion(String v1, String v2) {
    var components1 = getVersionComponents(v1);
    var components2 = getVersionComponents(v2);
    int length = Math.max(components1.length, components2.length);
    for (int i = 0; i < length; i++) {
      Integer c1 = i < components1.length ? Integer.parseInt(components1[i]) : 0;
      Integer c2 = i < components2.length ? Integer.parseInt(components2[i]) : 0;
      int result = c1.compareTo(c2);
      if (result != 0) {
        return result;
      }
    }
    return 0;
  }

  private static String[] getVersionComponents(String version) {
    return version.replaceAll(EXTRACT_VERSION_REGEX, "$1").split("\\.");
  }
}
```

### DuplicateCharacterSnippet

```java
package string;

import java.util.HashSet;
import java.util.Set;

/**
 * DuplicateCharacterSnippet.
 */
public class DuplicateCharacterSnippet {

  /**
   * Remove Duplicate Characters from a string.
   *
   * @param str The string to be processed
   * @return A string with no duplicate characters
   */

  public static String removeDuplicateCharacters(String str) {
    char[] charsOfStr = str.toCharArray();
    Set<String> uniqueCharacters = new HashSet<>();
    for (char character : charsOfStr) {
      uniqueCharacters.add(String.valueOf(character));
    }
    return String.join("", uniqueCharacters);
  }
}
```

### KmpSubstringSearchSnippet

```java
package string;

/**
 * KmpSubstringSearchSnippet.
 */

public class KmpSubstringSearchSnippet {
 
  /**
   * Implements the Knuth-Morris-Pratt (KMP) algorithm to find the of a substring.
   *
   * @param text The text in which the substring is to be searched.
   * @param pattern The substring pattern to search for.
   * @return The index of the first occurrence, or -1 if the pattern is not found.
   */
  public static int kmpSearch(String text, String pattern) {
    if (pattern == null || pattern.length() == 0) {
      return 0; // Trivial case: empty pattern
    }
 
    int[] lps = computeLpsArray(pattern);
    int i = 0; // index for text
    int j = 0; // index for pattern
 
    while (i < text.length()) {
      if (pattern.charAt(j) == text.charAt(i)) {
        i++;
        j++;
      }
 
      if (j == pattern.length()) {
        return i - j; // Found pattern at index (i - j)
      } else if (i < text.length() && pattern.charAt(j) != text.charAt(i)) {
        if (j != 0) {
          j = lps[j - 1]; // Use the LPS array to skip characters
        } else {
          i++; // If no match and j is 0, move to the next character in text
        }
      }
    }
    return -1; // Pattern not found
  }
 
  /**
  * Computes the LPS (Longest Prefix Suffix) array, which indicates the longest proper prefix.
  *
  * @param pattern The pattern for which the LPS array is to be computed.
  * @return The LPS array.
  */
  private static int[] computeLpsArray(String pattern) {
    int length = 0;
    int i = 1;
    int[] lps = new int[pattern.length()];
    lps[0] = 0; // LPS for the first character is always 0
 
    while (i < pattern.length()) {
      if (pattern.charAt(i) == pattern.charAt(length)) {
        length++;
        lps[i] = length;
        i++;
      } else {
        if (length != 0) {
          length = lps[length - 1]; // Fall back to the previous LPS value
        } else {
          lps[i] = 0;
          i++;
        }
      }
    }
    return lps;
  }
} 

### LevenshteinDistanceSnippet

```java
package string;

/**
 * LevenshteinDistanceSnippet.
 */
public class LevenshteinDistanceSnippet {

  /**
   * Find the Levenshtein distance between two words. https://en.wikipedia.org/wiki/Levenshtein_distance
   *
   * @param word1 first word
   * @param word2 second word
   * @return distance
   */
  public static int findLevenshteinDistance(String word1, String word2) {
    // If word2 is empty, removing
    int[][] ans = new int[word1.length() + 1][word2.length() + 1];
    for (int i = 0; i <= word1.length(); i++) {
      ans[i][0] = i;
    }
    // if word1 is empty, adding
    for (int i = 0; i <= word2.length(); i++) {
      ans[0][i] = i;
    }
    // None is empty
    for (int i = 1; i <= word1.length(); i++) {
      for (int j = 1; j <= word2.length(); j++) {
        int min = Math.min(Math.min(ans[i][j - 1], ans[i - 1][j]), ans[i - 1][j - 1]);
        ans[i][j] = word1.charAt(i - 1) == word2.charAt(j - 1) ? ans[i - 1][j - 1] : min + 1;
      }
    }
    return ans[word1.length()][word2.length()];
  }
}
```

### LSystemSnippet

```java
package string;

import java.util.Map;

/**
 * LSystemSnippet.
 */
public class LindenmayerSystemSnippet {
  /**
   * Generates an L-system string based on axiom, production rules, and a number of iterations.
   *
   * @param axiom           initial string to begin the L-system
   * @param productionRules map of character rules where each symbol can be replaced with a string
   * @param iterations      number of iterations to apply the production rules
   * @return the generated string after all iterations
   */
  public static String generateLindenmayerSystem(
          String axiom,
          Map<Character, String> productionRules,
          int iterations
  ) {
    String current = axiom;

    for (int i = 0; i < iterations; i++) {
      StringBuilder nextIteration = new StringBuilder(current.length() * 2);

      // Replace each symbol with the corresponding production rule or the symbol itself
      current.chars()
          .mapToObj(c -> (char) c)
          .forEach(symbol ->
                  nextIteration.append(
                          productionRules.getOrDefault(symbol, String.valueOf(symbol))
                  )
          );

      current = nextIteration.toString();
    }
    return current;
  }
}
```

### MaxCharacterCountSnippet

```java
package string;

/**
 * MaxCharacterCountSnippet.
 */
public class MaxCharacterCountSnippet {

  /**
   * The maximum count of times a specific character appears in a string.
   *
   * @param str َA specific string
   * @param character A specific character
   * @return the maximum count of one character
   */

  public static int getMaxCharacterCount(String str, char character) {
    int characterCount = 0;
    int maxCharacterCount = 0;
    for (int i = 0; i < str.length(); i++) {
      if ((str.charAt(i)) == character) {
        characterCount++;
        maxCharacterCount = Math.max(maxCharacterCount, characterCount);
      } else {
        characterCount = 0;
      }
    }
    return maxCharacterCount;
  }
}
```

### PalindromCheckSnippet

```java
package string;

/**
 * PalindromCheckSnippet.
 */
public class PalindromCheckSnippet {

  /**
   * Checks if given string is palindrome (same forward and backward). Skips non-letter characters
   * Credits: https://github.com/kousen/java_8_recipes
   *
   * @param s string to check
   * @return true if palindrome
   */
  public static boolean isPalindrome(String s) {
    for (int i = 0, j = s.length() - 1; i < j; i++, j--) {
      while (i < j && !Character.isLetter(s.charAt(i))) {
        i++;
      }
      while (i < j && !Character.isLetter(s.charAt(j))) {
        j--;
      }

      if (Character.toLowerCase(s.charAt(i)) != Character.toLowerCase(s.charAt(j))) {
        return false;
      }
    }

    return true;
  }
}
```

### ReversStringSnippet

```java
package string;

/**
 * ReversStringSnippet.
 */
public class ReverseStringSnippet {

  /**
   * Reverse string.
   *
   * @param s the string to reverse
   * @return reversed string
   */
  public static String reverseString(String s) {
    return new StringBuilder(s).reverse().toString();
  }
}
```

### StringToDateSnippet

```java
package string;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * StringToDateSnippet.
 */
public class StringToDateSnippet {

  /**
   * Convert string to date.
   *
   * @param date   the date string
   * @param format expected date format
   * @return Date
   * @throws ParseException in case of an unparseable date string
   */
  public static Date stringToDate(String date, String format) throws ParseException {
    var simpleDateFormat = new SimpleDateFormat(format);
    return simpleDateFormat.parse(date);
  }
}
```