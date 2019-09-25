---
layout: post
title: Python 内置函数 collections
category: python
tags: [python,collections]
---

# Python 内置函数 collections #

collections是Python内建的一个集合模块，提供了许多有用的集合类。

## collections ##

### 代码 ###

```python 
#!/usr/bin/env python3
# -*- coding: utf-8 -*-

print("-------------------namedtuple-------------------------")
from collections import namedtuple
Point = namedtuple('Point', ['x', 'y'])
p = Point(1, 2)
print(p.x)
print(p.y)
# 可以验证创建的Point对象是tuple的一种子类
print(isinstance(p, Point))
print(isinstance(p, tuple))

Circle = namedtuple('Circle', ['x', 'y', 'r'])
c = Circle(1, 2, 3)
print(c.x)
print(c.y)
print(c.r)

print("---------------------deque-----------------------")
from collections import deque
q = deque(['a', 'b', 'c'])
q.append('x')
q.appendleft('y')
print(q)

print("---------------------defaultdict------------------")
from collections import defaultdict
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
# key1存在
print(dd['key1'])
# key2不存在，返回默认值
print(dd['key2'])

print("---------------------OrderedDict------------------")
from collections import OrderedDict
d = dict([('a', 1), ('b', 2), ('c', 3)])
# dict的Key是无序的
print(d)
print(d.get('a'))
od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
# OrderedDict的Key是有序的
print(od)
print(od.get('a'))

od = OrderedDict()
od['z'] = 1
od['y'] = 2
od['x'] = 3
# 按照插入的Key的顺序返回
print(list(od.keys()))
print(list(od.values()))

print("---------------------ChainMap------------------")
from collections import ChainMap
import os, argparse

# 构造缺省参数:
defaults = {
    'color': 'red',
    'user': 'guest'
}

# 构造命令行参数:
parser = argparse.ArgumentParser()
parser.add_argument('-u', '--user')
parser.add_argument('-c', '--color')
namespace = parser.parse_args()
command_line_args = { k: v for k, v in vars(namespace).items() if v }

# 组合成ChainMap:
combined = ChainMap(command_line_args, os.environ, defaults)

# 打印参数:
print('color=%s' % combined['color'])
print('user=%s' % combined['user'])

print("---------------------Counter------------------")
# Counter是一个简单的计数器，例如，统计字符出现的个数：
from collections import Counter

c = Counter()
for ch in 'programming':
    c[ch] = c[ch] + 1

print(c)

```