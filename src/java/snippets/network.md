
### HttpGetSnippet

```java
package network;

import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;

/**
 * HttpGetSnippet.
 */
public class HttpGetSnippet {

  /**
   * Performs HTTP GET request.
   *
   * @param uri the URI of the connection
   * @return response object
   * @throws Exception i/o error, interruption error, etc
   */
  public static HttpResponse<String> httpGet(String uri) throws Exception {
    var client = HttpClient.newHttpClient();
    var request = HttpRequest.newBuilder()
            .uri(URI.create(uri))
            .build();
    return client.send(request, HttpResponse.BodyHandlers.ofString());
  }
}
```

### HttpPostSnippet

```java
package network;

import java.io.IOException;
import java.net.URI;
import java.net.URLEncoder;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.nio.charset.StandardCharsets;
import java.util.HashMap;
import java.util.StringJoiner;

/**
 * HttpPostSnippet.
 */
public class HttpPostSnippet {

  /**
   * Performs HTTP POST request. Credits https://stackoverflow.com/questions/3324717/sending-http-post-request-in-java
   *
   * @param address   the URL of the connection in String format, like "http://www.google.com"
   * @param arguments the body of the POST request, as a HashMap
   * @return response object
   * @throws IOException          if an I/O error occurs
   * @throws InterruptedException if the operation is interrupted
   */
  public static HttpResponse<String> httpPost(String address, HashMap<String, String> arguments)
          throws IOException, InterruptedException {
    var sj = new StringJoiner("&");
    for (var entry : arguments.entrySet()) {
      sj.add(URLEncoder.encode(entry.getKey(), StandardCharsets.UTF_8) + "="
              + URLEncoder.encode(entry.getValue(), StandardCharsets.UTF_8));
    }

    var out = sj.toString().getBytes(StandardCharsets.UTF_8);
    var request = HttpRequest.newBuilder()
            .uri(URI.create(address))
            .headers("Content-Type", "application/x-www-form-urlencoded; charset=UTF-8")
            .POST(HttpRequest.BodyPublishers.ofByteArray(out))
            .build();

    return HttpClient.newHttpClient().send(request, HttpResponse.BodyHandlers.ofString());
  }
}
```