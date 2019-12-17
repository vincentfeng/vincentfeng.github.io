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

## Micro Server ##

<https://github.com/vincentfeng/micro_repo/tree/master/micro_spark>

自定义的微服务，轻量级别，很多公司都是这么用，不考虑服务注册，抛弃Spring boot+Cloud
