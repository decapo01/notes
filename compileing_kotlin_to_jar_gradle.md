

in the `build.gradle` file put

```
jar {
    manifest {
        attributes 'Main-Class': 'com.packagename.AppKt'
    }
    from { configurations.compile.collect { it.isDirectory() ? it : zipTree(it) } }
}
```

where AppKt is the App.kt where the main class is located

then run

```
gradle clean jar
```