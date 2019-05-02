---
layout: _post
title: python高效编程技巧实战(二)
date: 2019-2-28 00:00:00
tags:
---


![](pyhton高效编程技巧实战-1/1.png)

<!--more-->

# 第二章 对象迭代与反迭代技巧训练
## 3-1 如何实现可迭代对象和迭代对象
### 从网络抓取城市气温信息并显示
#### 方法
https://blog.csdn.net/zoulonglong/article/details/80887244
- [collections.Iterable, Iterator](https://blog.csdn.net/passionkk/article/details/49929887)

``` 
# 迭代器对象
class WeatherIterator(Iterator):
    def __init__(self, cities):
        self.cities = cities
        self.index = 0

    def getWeather(self, city):
        '''返回 city 城市天气信息'''
        '''如: 北京: 最高气温: xx, 最低气温:xx'''
    def next(self):
        if self.index == len(self.cities):
            raise StopIteration
        city = self.cities[self.index]
        self.index += 1
        self.index += 1
        return self.getWeather(city)

# 可迭代对象
class WeatherIterable(Iterable):
    def __init__(self, cities):
        self.cities = cities
    def __iter__(self):
        return WeatherIterator(self.cities)

for x in WeatherIterable(['北京', '武汉',....]):
    print(x)
```

## 3-2 如何使用生成器函数实现可迭代对象
### 实现一个可迭代对象的类，他能迭代出给定范围内所有素数
#### 方法
- 让


```
class PrimeNumber:
    def __init__(self, start, end):
        self.start = start
        self.end = end

    def isPrime(self, k):
        for i in range(2, int(sqrt(k)+1))       # math.sqrt
            if k % i === 0
                return False
            return True
    def __iter__(self):
        for k in range(self.start, self.end+1):
            if self.isPrime(k):
                yield k

for x in PrimeNumber(1, 100):
    print(x)

```

## 3-3 如何进行反向迭代以及如何实现反向迭代
### 实现一个连续的浮点数发生器，根据给定的范围和步长产生一些列的浮点数(反向)

#### 方法
- reversed()

```
class FloatRange:
    def __init__(self, start, end,step=0.1):
        self.start = start
        self.end = end
        self.step = step
    def __iter__(self):
        t = self.start
        while t<= self.end:
            yield t
            t += self.step
    def __reversed__(self):
        t = self.end
        while t >= self.start:
            yield t
            t -= self.step
for x in FloatRange(1, 5, 0.5):
    print(x)
```
## 3-3 如何对迭代器做切片操作
### 有某个文本文件，我们想读取其中某范围的内容， 如100-300行之间的内容（python 中文本文件）

#### 方法

