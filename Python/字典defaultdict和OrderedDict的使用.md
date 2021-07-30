# 字典defaultdict和OrderedDict的使用
<p align="right">2021年8月，时间结余。</p>

---

## 1. defaultdict的使用

**需求：**

`bag = ['apple', 'orange', 'cherry', 'apple','apple', 'cherry', 'blueberry']`

用字典统计*bag*中*fruit*的数量。

**方法1：传统dict**

```python
bag = ['apple', 'orange', 'cherry', 'apple','apple', 'cherry', 'blueberry']
counts = {}
for fruit in bag:
    if fruit not in counts:
        counts[fruit] = 0
    counts[fruit] += 1
```

这种方法需要先判断key是否存在，不存在的话需要先初始化。

**方法1-1：传统dict**

```python
bag = ['apple', 'orange', 'cherry', 'apple','apple', 'cherry', 'blueberry']
counts = {}
for fruit in bag:
    counts.setdefault(fruit, 0)
    counts[fruit] += 1
```

这种方法调用dict的方法setdefault(key, value)来设置默认值。

> 如果key不存在，则插入key，赋值为value。方法返回值value。
>
> 如果key已经存在，不赋值，取出key对应的值value1。方法返回值value1。

**方法2：自带库collection中的defaultdict类**

```python
from collections import defaultdict
bag = ['apple', 'orange', 'cherry', 'apple','apple', 'cherry', 'blueberry']
counts = defaultdict(int)
for fruit in bag:
    counts[fruit] += 1
```

`counts = defaultdict(int)`完成了字典的初始化工作，值的类型为int。

## 2. OrderedDict和有序无序

字典的无序是指数据存进字典的顺序和取出字典的顺序不一致。

例如：

```python
dic = {'a':-1,'b':-1,'c':-1}
for k, v in dic.items():
    print(k, v)

output:
a -1
c -1
b -1
```

这个时候如果我们需要有序的话，就需要OrderedDict类。

**python3.6（包含）之后，所有的普通dict()字典都变为有序的了，不再需要OrderedDict这个类了。**

之前的版本如需要的话：

```python
from collections import OrderedDict
order_dict = OrderedDict()
order_dict['a'] = -1
order_dict['b'] = -1
order_dict['c'] = -1
for k, v in order_dict.items():
    print(k, v)

output:
a -1
b -1
c -1
```