---
layout: post
title: MuServer-初见
category: java
tags: [java,算法]
---

# MuServer - 初见 #

## 官网 ##

<https://muserver.io/>

## Quick Start ##

```xml 
<dependency>
    <groupId>io.muserver</groupId>
    <artifactId>mu-server</artifactId>
    <version>0.47.2</version>
</dependency>
```

```java
import io.muserver.*;
public class Hello {
    public static void main(String[] args) {
        MuServer server = MuServerBuilder.httpServer()
            .addHandler(Method.GET, "/", (request, response, pathParams) -> {
                response.write("Hello, world");
            })
            .start();
        System.out.println("Started server at " + server.uri());
    }
}
```

## Micro Server ##

<https://github.com/vincentfeng/micro_repo/tree/master/micro_muserver>

自定义的微服务，轻量级别，很多公司都是这么用，不考虑服务注册，抛弃Spring boot+Cloud