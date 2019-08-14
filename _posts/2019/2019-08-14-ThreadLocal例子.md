---
layout: post
title: ThreadLocal例子
category: java
tags: [java]
---

# ThreadLocal例子 #

## 用法 ##

1. ThreadLocal 线程共享的变量
2. ThreadLocal 不能多次创建

## 代码实现 ##

##### ThreadLocalUtil.java #####
``` java

package com.example.demo;

public class ThreadLocalUtil {

    private static ThreadLocal<String> requestId = new ThreadLocal<>();

    private static ThreadLocal<String> name = new ThreadLocal<>();

    public static String getRequestId(){
        return requestId.get();
    }

    public static void setRequestId(String s){
        requestId.set(s);
    }

    public static String getName(){
        return name.get();
    }

    public static void setName(String s){
        name.set(s);
    }
}

```

##### ThreadLocalDemo.java #####
``` java 
package com.example.demo;

public class ThreadLocalDemo {

    public static void main(String[] args) {

        String name = "Vincent.Fung";
        String requestId = "this is requestId";

        ThreadLocalUtil.setRequestId(requestId);
        ThreadLocalUtil.setName(name);
        // Main Thread
        t1();
        // Sub 1 Thread
        new Thread(new Runnable() {
            @Override
            public void run() {
                String name = "Sub 1 Vincent.Fung";
                String requestId = "Sub 1 this is requestId";
                ThreadLocalUtil.setRequestId(requestId);
                ThreadLocalUtil.setName(name);
                t2();
            }
        }).start();
        // Sub 2 Thread
        new Thread(new Runnable() {
            @Override
            public void run() {
                String name = "Sub 2 Vincent.Fung";
                String requestId = "Sub 2 this is requestId";
                ThreadLocalUtil.setRequestId(requestId);
                ThreadLocalUtil.setName(name);
                t2();
            }
        }).start();
    }

    public static void t1(){
        // Main Thread
        System.out.println(ThreadLocalUtil.getName());
        System.out.println(ThreadLocalUtil.getRequestId());
    }

    public static void t2(){
        // Sub Thread
        System.out.println(ThreadLocalUtil.getName());
        System.out.println(ThreadLocalUtil.getRequestId());
    }

}


```