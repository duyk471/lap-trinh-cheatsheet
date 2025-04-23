# CLS

### CreatingObjectSnippet

```java
package cls;

import java.lang.reflect.InvocationTargetException;

/**
 * CreatingObjectSnippet.
 */
public class CreatingObjectSnippet {

  /**
   * Create object using reflection.
   *
   * @param cls fully qualified name of class includes the package name as String
   * @return object
   * @throws NoSuchMethodException if a method that does not exist at runtime.
   * @throws IllegalAccessException <p>if an currently executing method does not have access to
   *     the definition of the specified class, field, method or constructor</p>
   * @throws InvocationTargetException <p>InvocationTargetException is a checked exception
   *     that wraps an exception thrown by an invoked method or constructor.</p>
   * @throws InstantiationException <p>when an method tries to create an instance of a class
   *     using the newInstance method in class Class.</p>
   * @throws ClassNotFoundException <p>when an application tries to load in a class
   *     through its string name.</p>
   */
  public static Object createObject(String cls)
          throws NoSuchMethodException,
          IllegalAccessException,
          InvocationTargetException,
          InstantiationException,
          ClassNotFoundException {
    var objectClass = Class.forName(cls);
    var objectConstructor = objectClass.getConstructor();
    return objectConstructor.newInstance();
  }
}
```

### GetAllFieldNamesSnippet

```java
package cls;

import java.lang.reflect.Field;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

/**
 * GetAllFieldNamesSnippet.
 */
public class GetAllFieldNamesSnippet {

  /**
   * Print all declared field names of the class or the interface the class extends.
   *
   * @param clazz Tested class
   * @return list of names of all fields
   */
  public static List<String> getAllFieldNames(final Class<?> clazz) {
    var fields = new ArrayList<String>();
    var currentClazz = clazz;
    while (currentClazz != null) {
      fields.addAll(
          Arrays.stream(currentClazz.getDeclaredFields())
              .filter(field -> !field.isSynthetic())
              .map(Field::getName)
              .collect(Collectors.toList()));
      currentClazz = currentClazz.getSuperclass();
    }
    return fields;
  }
}
```

### GetAllMethodsSnippet

```java
package cls;

import java.lang.reflect.Method;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

/**
 * GetAllMethodsSnippet.
 */
public class GetAllMethodsSnippet {

  /**
   * Print all declared methods of the class.
   *
   * @param cls Tested class
   * @return list of methods name
   */
  public static List<String> getAllMethods(final Class<?> cls) {
    return Arrays.stream(cls.getDeclaredMethods())
        .map(Method::getName)
        .collect(Collectors.toList());
  }
}
```

### GetAllPublicFieldNamesSnippet

```java
package cls;

import java.lang.reflect.Field;
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

/**
 * GetAllPublicFieldNamesSnippet.
 */
public class GetAllPublicFieldNamesSnippet {

  /**
   * Print all declared public field names of the class or the interface the class extends.
   *
   * @param clazz Tested class
   * @return list of name of public fields
   */
  public static List<String> getAllPublicFieldNames(final Class<?> clazz) {
    return Arrays.stream(clazz.getFields())
        .map(Field::getName)
        .collect(Collectors.toList());
  }
}
```