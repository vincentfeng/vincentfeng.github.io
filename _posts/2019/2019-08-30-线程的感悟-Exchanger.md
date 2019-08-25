---
layout: post
title: 线程的感悟-Exchanger
category: java
tags: [java,线程]
---

# 线程的感悟-Exchanger #

## 简介 ##

java.util.concurrent包中的Exchanger类可用于两个线程之间交换信息。可简单地将Exchanger对象理解为一个包含两个格子的容器，通过exchanger方法可以向两个格子中填充信息。当两个格子中的均被填充时，该对象会自动将两个格子的信息交换，然后返回给线程，从而实现两个线程的信息交换。

注意了，结对出现，如果有线程找不到交换的数据，就会出现阻塞，永久等待，直到与之配对的线程到达位置。

## 代码 ##
``` java 
import java.util.concurrent.Exchanger;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExchangerTest {

    public static void main(String[] args) {
        ExecutorService service = Executors.newCachedThreadPool();
        final Exchanger exchanger = new Exchanger();
        service.execute(new Runnable(){
            public void run() {
                try {

                    String data1 = "Thread1";
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "正在把数据" + data1 +"换出去");
                    Thread.sleep((long)(Math.random()*10000));
                    String data2 = (String)exchanger.exchange(data1);
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "换回的数据为" + data2);
                }catch(Exception e){

                }
            }
        });
        service.execute(new Runnable(){
            public void run() {
                try {

                    String data1 = "Thread2";
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "正在把数据" + data1 +"换出去");
                    Thread.sleep((long)(Math.random()*10000));
                    String data2 = (String)exchanger.exchange(data1);
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "换回的数据为" + data2);
                }catch(Exception e){

                }
            }
        });
        service.execute(new Runnable(){
            public void run() {
                try {

                    String data1 = "Thread3";
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "正在把数据" + data1 +"换出去");
                    Thread.sleep((long)(Math.random()*10000));
                    String data2 = (String)exchanger.exchange(data1);
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "换回的数据为" + data2);
                }catch(Exception e){

                }
            }
        });
        service.execute(new Runnable(){
            public void run() {
                try {

                    String data1 = "Thread4";
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "正在把数据" + data1 +"换出去");
                    Thread.sleep((long)(Math.random()*10000));
                    String data2 = (String)exchanger.exchange(data1);
                    System.out.println("线程" + Thread.currentThread().getName() +
                            "换回的数据为" + data2);
                }catch(Exception e){

                }
            }
        });
    }
}

```