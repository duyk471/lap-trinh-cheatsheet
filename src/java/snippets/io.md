# IO

### InputStreamToStringSnippet

```java
package io;

import java.io.BufferedReader;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.nio.charset.StandardCharsets;
import java.util.stream.Collectors;

/**
 * InputStreamToStringSnippet.
 */
public class InputStreamToStringSnippet {

  /**
   * Convert InputStream to String.
   *
   * @param inputStream InputStream to convert
   * @return String
   */
  public static String inputStreamToString(InputStream inputStream) {
    return new BufferedReader(new InputStreamReader(inputStream, StandardCharsets.UTF_8))
            .lines().collect(Collectors.joining(System.lineSeparator()));
  }
}
```

### ReadFileSnippet

```java
package io;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

/**
 * ReadFileSnippet.
 */
public class ReadFileSnippet {

  /**
   * Read file using stream and return list of string lines.
   *
   * @param fileName file to read
   * @throws FileNotFoundException if an I/O error occurs
   */
  public static List<String> readFile(String fileName) throws FileNotFoundException {
    try (Stream<String> stream = new BufferedReader(new FileReader(fileName)).lines()) {
      return stream.collect(Collectors.toList());
    }
  }
}

```