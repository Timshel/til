# Different Thread Pool

## Fixed thread pool :

For most cases don't forget to define a custom Factory and set Thread as daemon.
This way even if shutdown is not called it will not block JVM shutdown.

```scala
object DaemonThreadFactory extends ThreadFactory {
  override def newThread(r: Runnable): Thread = {
    val t = new Thread(r)
    t.setDaemon(true)
    t
  }
}
```

## Fork join pool :

Thread are defined as daemon as default in the Pool Cf: https://docs.oracle.com/javase/7/docs/api/java/util/concurrent/ForkJoinPool.html
