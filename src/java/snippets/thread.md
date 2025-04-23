
### ThreadPool

```java
package thread;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

/**
 * ThreadPool.
 */
public class ThreadPool {

  /**
   * <p>Creates pool of threads. Where the pool is the size of the number of processors
   * available to the Java virtual machine.</p>
   *
   * @return the newly created thread pool
   */
  public static ExecutorService createFixedThreadPool() {
    return Executors.newFixedThreadPool(Runtime.getRuntime().availableProcessors());
  }
}
```

### ThreadSnippet

```java
package thread;

/**
 * ThreadSnippet.
 */
public class ThreadSnippet {

  /**
   * Creates and returns a new thread with the task assigned to it
   * (task will be performed parallel to the main thread).
   *
   * @param task the task to be executed by this thread
   * @return new thread with task assigned to it.
   */
  public static Thread createThread(Runnable task) {
    return new Thread(task);
  }
}
```