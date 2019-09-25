---
layout: post
title: Python 内置函数 Datetime
category: python
tags: [python,datetime]
---

# Python 内置函数 Datetime #

datetime是Python处理日期和时间的标准库。

## datetime ##

```python 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

from datetime import datetime, timedelta, timezone

now =datetime.now()
print(now)
print(type(now))
print(now.strftime('%a, %b %d %H:%M'))

dt = datetime(2015,12,20,15,17,59)
print(dt)
print(dt.timestamp())

t = 1429417200.0
# Local time
print(datetime.fromtimestamp(t))
# UTC time
print(datetime.utcfromtimestamp(t))

cday = datetime.strptime('2015-6-1 18:19:59', '%Y-%m-%d %H:%M:%S')
print(cday)

#datetime加减
print(now + timedelta(hours=20))
print(now - timedelta(hours=20))
print(now - timedelta(days=1))
print(now + timedelta(days=2, hours=12))

# 创建时区UTC+8:00
tz_utc_8 = timezone(timedelta(hours=8))
print(datetime.now())
# 强制设置为UTC+8:00
print(datetime.now().replace(tzinfo=tz_utc_8))

print("-------------------------------------")
# 拿到UTC时间，并强制设置时区为UTC+0:00:
utc_dt = datetime.utcnow().replace(tzinfo=timezone.utc)
print(utc_dt)

# astimezone()将转换时区为北京时间:
bj_dt = utc_dt.astimezone(timezone(timedelta(hours=8)))
print(bj_dt)

# astimezone()将转换时区为东京时间:
tokyo_dt = utc_dt.astimezone(timezone(timedelta(hours=9)))
print(tokyo_dt)

# astimezone()将bj_dt转换时区为东京时间:
tokyo_dt2 = bj_dt.astimezone(timezone(timedelta(hours=9)))
print(tokyo_dt2)
```