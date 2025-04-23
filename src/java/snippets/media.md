# Media

### CaptureScreenSnippet

```java
package media;

import java.awt.AWTException;
import java.awt.Rectangle;
import java.awt.Robot;
import java.awt.Toolkit;
import java.io.File;
import java.io.IOException;
import javax.imageio.ImageIO;

/**
 * CaptureScreenSnippet.
 */
public class CaptureScreenSnippet {

  /**
   * Capture screenshot and save it to PNG file. Credits: https://viralpatel.net/blogs/how-to-take-screen-shots-in-java-taking-screenshots-java/
   *
   * @param filename the name of the file
   * @throws AWTException if the platform configuration does not allow low-level input control
   * @throws IOException  if an I/O error occurs
   */
  public static void captureScreen(String filename) throws AWTException, IOException {
    var screenSize = Toolkit.getDefaultToolkit().getScreenSize();
    var screenRectangle = new Rectangle(screenSize);
    var robot = new Robot();
    var image = robot.createScreenCapture(screenRectangle);
    ImageIO.write(image, "png", new File(filename));
  }
}
```