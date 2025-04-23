
### Base64DecodeSnippet

```java
package encoding;

import java.util.Base64;

/**
 * Base64DecodeSnippet.
 */
public class Base64DecodeSnippet {

  /**
   * Decodes a Base64 encoded string to the actual representation.
   *
   * @param input base64 encoded string
   * @return decoded string
   */
  public static String decodeBase64(String input) {
    return new String(Base64.getDecoder().decode(input.getBytes()));
  }
}
```

### Base64EncodeSnippet

```java
package encoding;

import java.util.Base64;

/**
 * Base64EncodeSnippet.
 */
public class Base64EncodeSnippet {
  /**
   * Encodes the input string to a Base64 encoded string.
   *
   * @param input string to be encoded
   * @return base64 encoded string
   */
  public static String encodeBase64(String input) {
    return Base64.getEncoder().encodeToString(input.getBytes());
  }
}

```