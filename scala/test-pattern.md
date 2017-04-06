# Write a test with Pattern matching

## Scalatest

```scala
import org.scalatest.Inside.inside

inside(validated) { case Valid(d) => assert(d === delivery) }
```

## Specs2

```scala
beLike { case exp => result }
```

