---
layout: post
title: 线程的感悟-ThreadLocal
category: java
tags: [java,线程]
---

# ThreadLocal例子 #

## 用法 ##

1. ThreadLocal 线程共享的变量
2. ThreadLocal 不能多次创建

## 代码实现<一> ##

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

## 代码实现<二> ##

通过代码实现<一> 可以看到，需要用一个全局的ThreadLocal 去存放对象，才可以从JVM存活的过程中获取独立线程的独立数据。

``` java 
import java.util.HashMap;
import java.util.Map;
import java.util.Random;

public class ThreadLocalTest {

    private static ThreadLocal<Integer> x = new ThreadLocal<Integer>();
    private static ThreadLocal<MyThreadScopeData> myThreadScopeData = new ThreadLocal<MyThreadScopeData>();
    public static void main(String[] args) {
        for(int i=0;i<2;i++){
            new Thread(new Runnable(){
                @Override
                public void run() {
                    int data = new Random().nextInt();
                    System.out.println(Thread.currentThread().getName()
                            + " has put data :" + data);
                    x.set(data);
                    MyThreadScopeData.getThreadInstance().setName("name" + data);
                    MyThreadScopeData.getThreadInstance().setAge(data);
                    new A().get();
                    new B().get();
                }
            }).start();
        }
    }

    static class A{
        public void get(){
            int data = x.get();
            System.out.println("A from " + Thread.currentThread().getName()
                    + " get data :" + data);
            MyThreadScopeData myData = MyThreadScopeData.getThreadInstance();
            System.out.println("A from " + Thread.currentThread().getName()
                    + " getMyData: " + myData.getName() + "," +
                    myData.getAge());
        }
    }

    static class B{
        public void get(){
            int data = x.get();
            System.out.println("B from " + Thread.currentThread().getName()
                    + " get data :" + data);
            MyThreadScopeData myData = MyThreadScopeData.getThreadInstance();
            System.out.println("B from " + Thread.currentThread().getName()
                    + " getMyData: " + myData.getName() + "," +
                    myData.getAge());
        }
    }
}

class MyThreadScopeData{
    private MyThreadScopeData(){}
    public static MyThreadScopeData getThreadInstance(){
        MyThreadScopeData instance = map.get();
        if(instance == null){
            instance = new MyThreadScopeData();
            map.set(instance);
        }
        return instance;
    }

    private static ThreadLocal<MyThreadScopeData> map = new ThreadLocal<MyThreadScopeData>();

    private String name;
    private int age;
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```