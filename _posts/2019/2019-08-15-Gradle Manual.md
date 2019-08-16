---
layout: post
title: Gradle Manual
category: gradle
tags: [gradle]
---

# Gradle Manual #

https://guides.gradle.org

https://gradle.org/guides/#getting-started

gradle 本来我真的不想用 ， 但是看了同事用得很溜很溜。心里痒痒的，于是，我尝试一下咯。
看样子真的比maven好用。 

## gradle install ##

1. install java 
2. download gradle.zip
3. set environment
4. gradle -v

## upgrade to gradlew ##

1. gradle wrapper
2. gradlew.bat wrapper --gradle-version=5.6 --distribution-type=bin
3. gradlew.bat tasks

gradlew 其实真的好，简简单单包装一下。就可以兼容gradle 每个版本的升级。

## gradlew the first task ##

``` gradle script
task helloworld{
   print("hello world")
}
```

``` gradle script 
task copy(type: Copy, group: "Custom", description: "Copies sources to the dest directory") {
    from "src"
    into "dest"
}
```

``` gradle script 
task zip(type: Zip, group: "Archive", description: "Archives sources in a zip file") {
    from "src"
    setArchiveName "basic-demo-1.0.zip"
}
```

如果你想执行task ， 可以尝试

``` 
gradlew.bat <task name>
```

