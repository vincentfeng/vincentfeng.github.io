---
layout: post
title: Spark Web Server-另一个Micro Web
category: java
tags: [java,算法]
---

# Spark - 初见 #

## 官网 ##

<http://sparkjava.com/>

## Quick Start ##

```xml 
<dependencies>
    <dependency>
        <groupId>com.sparkjava</groupId>
        <artifactId>spark-core</artifactId>
        <version>2.5</version>
    </dependency>
</dependencies>
```

```java
import static spark.Spark.*;

public class Main {
    public static void main(String[] args) {
        get("/hello", (req, res) -> "Hello World");
        System.out.println("http://localhost:"+port());
    }
}
```
