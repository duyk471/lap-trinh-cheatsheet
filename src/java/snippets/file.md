
### ListAllFilesSnippet

```java
package file;

import java.io.File;
import java.util.ArrayList;
import java.util.List;

/**
 * ListAllFilesSnippet.
 */
public class ListAllFilesSnippet {

  /**
   * Recursively list all the files in directory.
   *
   * @param path the path to start the search from
   * @return list of all files
   */
  public static List<File> listAllFiles(String path) {
    var all = new ArrayList<File>();
    var list = new File(path).listFiles();
    if (list != null) {  // In case of access error, list is null
      for (var f : list) {
        if (f.isDirectory()) {
          all.addAll(listAllFiles(f.getAbsolutePath()));
        } else {
          all.add(f.getAbsoluteFile());
        }
      }
    }
    return all;
  }
}
```

### ListDirectoriesSnippet

```java
package file;

import java.io.File;

/**
 * ListDirectoriesSnippet.
 */
public class ListDirectoriesSnippet {

  /**
   * List directories.
   *
   * @param path the path where to look
   * @return array of File
   */
  public static File[] listDirectories(String path) {
    return new File(path).listFiles(File::isDirectory);
  }
}
```

### ListFilesInDirectorySnippet

```java
package file;

import java.io.File;

/**
 * ListFilesInDirectorySnippet.
 */
public class ListFilesInDirectorySnippet {

  /**
   * List files in directory.
   *
   * @param folder the path where to look
   * @return array of File
   */
  public static File[] listFilesInDirectory(String folder) {
    return new File(folder).listFiles(File::isFile);
  }
}
```

### ReadLinesSnippet

```java
package file;

import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

/**
 * ReadLinesSnippet.
 */
public class ReadLinesSnippet {

  /**
   * Read file as list of strings.
   *
   * @param filename the filename to read from
   * @return list of strings
   * @throws IOException if an I/O error occurs
   */
  public static List<String> readLines(String filename) throws IOException {
    return Files.readAllLines(Paths.get(filename));
  }
}
```

### ZipDirectorySnippet

```java
package file;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

/**
 * ZipDirectorySnippet.
 */
public class ZipDirectorySnippet {

  /**
   * Zip a complete directory.
   *
   * @param srcDirectoryName The path to the directory to be zipped
   * @param zipFileName The location and name of the zipped file.
   * @throws IOException if an I/O error occurs
   * */
  public static void zipDirectory(String srcDirectoryName, String zipFileName) throws IOException {
    var srcDirectory = new File(srcDirectoryName);
    try (
        var fileOut = new FileOutputStream(zipFileName);
        var zipOut = new ZipOutputStream(fileOut)
    ) {
      zipFile(srcDirectory, srcDirectory.getName(), zipOut);
    }
  }

  /**
   * Utility function which either zips a single file, or recursively calls itself for 
   * a directory to traverse down to the files contained within it.
   *
   * @param fileToZip The file as a resource
   * @param fileName The actual name of the file
   * @param zipOut The output stream to which all data is being written
   * */
  public static void zipFile(File fileToZip, String fileName, ZipOutputStream zipOut) 
      throws IOException {
    if (fileToZip.isHidden()) { // Ignore hidden files as standard
      return;
    }
    if (fileToZip.isDirectory()) {
      if (fileName.endsWith("/")) {
        zipOut.putNextEntry(new ZipEntry(fileName)); // To be zipped next
        zipOut.closeEntry();
      } else {
        // Add the "/" mark explicitly to preserve structure while unzipping action is performed
        zipOut.putNextEntry(new ZipEntry(fileName + "/"));
        zipOut.closeEntry();
      }
      var children = fileToZip.listFiles();
      for (var childFile : children) { // Recursively apply function to all children
        zipFile(childFile, fileName + "/" + childFile.getName(), zipOut);
      }
      return;
    }
    try (
        var fis = new FileInputStream(fileToZip) // Start zipping once we know it is a file
    ) {
      var zipEntry = new ZipEntry(fileName);
      zipOut.putNextEntry(zipEntry);
      var bytes = new byte[1024];
      var length = 0;
      while ((length = fis.read(bytes)) >= 0) {
        zipOut.write(bytes, 0, length);
      }
    }
  }
}
```

### ZipFileSnippet

```java
package file;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

/**
 * ZipFileSnippet.
 */
public class ZipFileSnippet {

  /**
   * Zip single file.
   *
   * @param srcFilename the filename of the source file
   * @param zipFilename the filename of the destination zip file
   * @throws IOException if an I/O error occurs
   */
  public static void zipFile(String srcFilename, String zipFilename) throws IOException {
    var srcFile = new File(srcFilename);
    try (
            var fileOut = new FileOutputStream(zipFilename);
            var zipOut = new ZipOutputStream(fileOut);
            var fileIn = new FileInputStream(srcFile)
    ) {
      var zipEntry = new ZipEntry(srcFile.getName());
      zipOut.putNextEntry(zipEntry);
      final var bytes = new byte[1024];
      int length;
      while ((length = fileIn.read(bytes)) >= 0) {
        zipOut.write(bytes, 0, length);
      }
    }
  }
}
```

### ZipFilesSnippet


```java
package file;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.zip.ZipEntry;
import java.util.zip.ZipOutputStream;

/**
 * ZipFilesSnippet.
 */
public class ZipFilesSnippet {

  /**
   * Zip multiples files.
   *
   * @param srcFilenames array of source file names
   * @param zipFilename  the filename of the destination zip file
   * @throws IOException if an I/O error occurs
   */
  public static void zipFiles(String[] srcFilenames, String zipFilename) throws IOException {
    try (
            var fileOut = new FileOutputStream(zipFilename);
            var zipOut = new ZipOutputStream(fileOut)
    ) {
      for (String srcFilename : srcFilenames) {
        var srcFile = new File(srcFilename);
        try (var fileIn = new FileInputStream(srcFile)) {
          var zipEntry = new ZipEntry(srcFile.getName());
          zipOut.putNextEntry(zipEntry);
          final var bytes = new byte[1024];
          int length;
          while ((length = fileIn.read(bytes)) >= 0) {
            zipOut.write(bytes, 0, length);
          }
        }
      }
    }
  }
}
```