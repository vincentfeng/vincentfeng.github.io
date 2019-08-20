---
layout: post
title: Bootstrap Table
category: javascript
tags: [javascript,bootstrap]
---

# BootStrap table #

## Home ##
https://bootstrap-table.com/  如果点进去看懂了，就不要往下看了。看不懂的话，往下看也没有什么意思。

## 笔记 ##

这里的笔记是曾经做过的demo，说实话，官方的例子才是王道。

### Demo ###

做一个简单的web，看自己怎么方便怎么做吧

#### index.html ####
```html

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.3/css/all.css" integrity="sha384-UHRtZLI+pbxtHCWp1t77Bi1L4ZtiqrqD80Kn4Z8NTSRyMA2Fd33n5dQ8lWUE00s/" crossorigin="anonymous">
    <link href="https://unpkg.com/bootstrap-table@1.15.4/dist/bootstrap-table.min.css" rel="stylesheet">
</head>
<body>
<!--
data-url="https://examples.wenzhixin.net.cn/examples/bootstrap_table/data"
data-pagination="true"
       data-side-pagination="server"

-->
<table id="table"
       data-toggle="table"
       data-url="/getdata"
       data-search="true"
       data-query-params="queryParams"

       class="table-striped table-sm"
>
    <thead>
    <tr>
        <th data-field="id" data-sortable="true">ID</th>
        <th data-field="name" data-sortable="true">Item Name</th>
        <th data-field="age" data-sortable="true">Item Age</th>
    </tr>
    </thead>
</table>
</body>
<script src="https://code.jquery.com/jquery-3.3.1.min.js" integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.6/umd/popper.min.js" integrity="sha384-wHAiFfRlMFy6i5SRaxvfOCifBUQy1xHdJ/yoi7FRNXMRBu5WHdZYu1hA6ZOblgut" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/js/bootstrap.min.js" integrity="sha384-B0UglyR+jN6CkvvICOB2joaf5I4l3gm9GU6Hc1og6Ls7i6U/mkkaduKaBhlAXv9k" crossorigin="anonymous"></script>
<script src="https://unpkg.com/bootstrap-table@1.15.4/dist/bootstrap-table.min.js"></script>
<script>
    var $table = $('#table')

    function queryParams(params) {
        console.log('queryParams: ' + JSON.stringify(params))
        return params
    }

    $(function() {
        $('.toolbar input').change(function () {
            var queryParamsType = $('.toolbar input:checked').next().text()

            $table.bootstrapTable('refreshOptions', {
                queryParamsType: queryParamsType
            })
        })
    })
</script>
</html>
```
data-url 可以选择 官方的 https://examples.wenzhixin.net.cn/examples/bootstrap_table/data ，也可以自己写一个。
但是自己写的时候，注意一点，请参考官方的格式。

#### 自己写提供数据的java类 ####
``` java
package com.example.demo;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ResponseBody;
import org.springframework.web.bind.annotation.RestController;

import java.util.ArrayList;
import java.util.HashMap;

@RestController
public class DataCtrl {

    @GetMapping("/getdata")
    @ResponseBody
    public ArrayList<HashMap<String,String>> getData(){
        ArrayList<HashMap<String,String>> list = new ArrayList<>();
        for(int i = 1 ; i <= 100 ; i++){
            HashMap<String,String> map = new HashMap<>();
            map.put("id",String.valueOf(i));
            map.put("name","[Vincent]"+i);
            map.put("age",String.valueOf(Integer.valueOf((int)(Math.random()*100))));
            list.add(map);
        }
        return list;
    }

}

```

## 感受 ##

这个算比较简单的table了，我之前用过的table 不下100 个。网上都不一定能找到100个。因为我自己实现就做了10个。
最经典的还是用java+xml+sqlserver 的版本。神级，可惜太不美观，而且企业化比较严重。但是真的非常方便。一条sql，就可以把数据表证生成。