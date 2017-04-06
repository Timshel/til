# Access sbt.Logger in Build.sbt

The access to the logger `state.value.log` need to be wrapped in a `Def.taskDyn`.

```scala
play.PlayImport.PlayKeys.playRunHooks ++= (
  Def.taskDyn {
    baseDirectory.map { base => BuildWebApp(base, state.value.log) }
  }
).value
```
