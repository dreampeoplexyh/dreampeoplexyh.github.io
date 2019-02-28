---
title: python 高效编程技巧实战
date: 2019-02-28 00:00:00
tags:
---
![](python高效编程技巧实战/1.png)

<!--more-->


# 第二章 数据结构与算法进阶训练
## 2-1 如何在列表，字典，集合中根据条件筛选数据
### 过滤列表的负数
```
data=[randint(-10, 10) for _ in range(10)]      # [4, -2, 0, -1, -6, -3, -10, -9, 0, 3]
```

#### 方法

- `1、filter(lambda x:x>0, data)`

- `2、[x for x in data if x>=0]`

### 筛选字典某些元素
```
stu = {x : randint(60,101) for x in range(1, 21)}   #{1: 73, 2: 68, 3: 62, 4: 78, 5: 94, 6: 97, 7: 62, 8: 93, 9: 90, 10: 95, 11: 71, 12: 79, 13: 66, 14: 60, 15: 98, 16: 95, 17: 86, 18: 94, 19: 62, 20: 87}
```

#### 方法
- `{k；v for k, v in stu.items() if v >= 90}`
  
### 筛选集合中某些元素
`s = set(data)       # data为上面生成的列表`

#### 方法
- `{x for x in s if s%3 == 0}`

### 筛处集合中某些元素
`s = set(data)     # data为上面生成的列表`

#### 方法
- `{x for x in s if x % 3 ==0}`






## 2-2 如何为元组中的每个元素命名，提高c程序的可读性

`stu  = ('Jim', 16, 'male', 'jim@gmail.com')`

#### 方法:
- `1、name, age, sex, male = range(4)                  # 访问： stu[name]`

- `2、collections.namedtuple`

```
Student = namedtuple('Student', ['name', 'age', 'sex', 'mail'])
s = Student('Jim', 16, 'male', 'jim@gmail.com')
s.name # 'Jim'
isinstance(s, tuple) # True
```


## 2-3 如何统计序列中元素的出现频度

### 随机序列出现次数最多的3个数
#### 方法：
- collections.Counter

```
data = [randint(0, 10) for _ in range(30)] 
# [1, 9, 1, 9, 4, 7, 8, 3, 4, 6, 3, 9, 0, 9, 2, 2, 6, 9, 8, 1, 4, 5, 7, 0, 5, 8, 3, 9, 0, 9]
c  = Counter(data) 
c.most_common(3) # [(9, 7), (0, 3), (1, 3)]
```


### 对英文文本进行单词词频统计，找到频率最高的10个单词
#### 方法：
```
colletcions.Counter
txt = open('test.txt').read()
c = Counter(re.split('\W', txt))            # 分割
c.most_common(10)
```


## 2-4 如何根据字典中值的大小，对字典中的项排排序
{'张三'：89, '李四':98}

#### 方法:
- 1、利用zip函数，将字典改成值在前，键在后(即 成绩在前，名字在后)

```
d =  {x:randint(60, 100) for x in 'xyzabc'}
sorted(zip(d.values(), d.keys()), reverse = True)
```

- 2、传递sorted函数的key函数key参数
```
sorted(d.items(), key = lambda x:x[1])
```


## 2-5 如何快速找到多个字典中的公共键（key）
{'a':1, 'b':1}, {'b':1, 'c':2}, {'a':1, 'd':2}...
#### 方法
- 利用集合(set)的交集操作
```
from random import randint, sample
s1 = {x:randint(1, 4) for x in sample('abcdefg', randint(3, 6))}
s2 = {x:randint(1, 4) for x in sample('abcdefg', randint(3, 6))}
s3 = {x:randint(1, 4) for x in sample('abcdefg', randint(3, 6))}
s1.keys() & s2.keys() & s3.keys() # 得到交集
# 若是有 n 个集合
map(dict.keys, [s1, s2, s3...]) 
from functools import reduce # python3
reduce(lambda a, b:a & b, map(dict.keys, [s1, s2, s3...]))
# m = map(dict.keys, [s1, s2, s3]) m 是 map 对象, list(m)对其访问
```

## 2-6 如何让字典保持有序
### 方法
- collections.OrdereDict


``` 
d = {}
d['Jim'] = (1, 35)
d['Tom'] = (2, 37)
d['Bob'] = (3, 42)
for k in d: print(k) 
# Jim  Bob  Tom 并不是按照进入字典序列打印
from collections import OrderedDict
d = OrderedDict()
d['Jim'] = (1, 35)
d['Tom'] = (2, 37)
d['Bob'] = (3, 42)
for k in d: print(k) 
# Jim Tom Bob

```

## 2-7 实现用户的历史记录
### 猜字游戏，显示之前输入的数
#### 方法
- colletions.deque 双端循环对垒，使用pickle储存python对象，以便下次加载
```
q = deque([], 10) # 容量为5
for i in range(6, 10):
    q.append(i)
pickle.dump(q, open('histor', 'wb'))
q2 = pickle.load(open('histor', 'rb'))
print(q2)           # deque([6, 7, 8, 9], maxlen=10)
```

