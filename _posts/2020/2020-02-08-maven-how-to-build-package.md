---
layout: post
title: How to build a package in Maven
category: java
tags: [java]
---

在Maven中，主要有3个插件可以用来打包：

1. maven-jar-plugin，默认的打包插件，用来打普通的project JAR包；

2. maven-shade-plugin，用来打可执行JAR包，也就是所谓的fat JAR包；

3. maven-assembly-plugin，支持自定义的打包结构，也可以定制依赖项等。
