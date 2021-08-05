# json模块，4个方法

<p align="right">2021年8月，时间结余。</p>

---
`json`模块源码：[github](https://github.com/zhhl9101/cpython/blob/main/Lib/json/__init__.py)

4个方法：`json.load()`，`json.loads()`， `json.dump()`，`json.dumps()`

望文生义，`load`即“加载”，读操作，将字符串转为`json`；`dump`即“转储”，写操作，将`json`转为字符串。

---

## 1. 读：~~`json.load()`~~，`json.loads()`

方法参数：

`json.load(fp, *, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw)`

`json.loads(s, *, cls=None, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, object_pairs_hook=None, **kw)`

区别：

`load()`读的是对象`fp`，这个对象得支持 `.read()`方法，常用的是文件对象。

`loads()`，即`load string`，读的是字符串`s`。

看源码可以知道，`load()`方法的实现其实是调用`loads()`方法，第一个参数即为`fp.read()`

 <img src="..\pictures\python\load.jpg" title="load" width="600px" height="500px">

所有这2个方法是差不多的，我们没必要直接用`load(fp)`，可以直接把对象`fp`处理成字符串再用`loads(s)`即可。

`load()`举例：

```python
import json  #"json"全小写

with open('data.json', 'r', encoding='utf-8') as fp:
    data = json.load(fp)
```

简洁的写法：

```python 
import json  #"json"全小写

data = json.load(open('data.json', 'r', encoding='utf-8'))  #读的是文件对象
```

也可以改用`loads()`:

```python
import json  #"json"全小写

data = json.loads(open('data.json', 'r', encoding='utf-8').read())  #读的是字符串
```

## 2. 写：`json.dump()`，`json.dumps()`

方法参数：

`json.dump(obj, fp, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, sort_keys=False, **kw)`

`json.dumps(obj, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, indent=None, separators=None, default=None, sort_keys=False, **kw)`

区别：

`dump()`将`json`对象`obj`写入到对象`fp`，常用的是写入到文件。

`dumps()`将`json`对象`obj`转化为字符串。

示例：

```python
import json  #"json"全小写

data = {'name':'Tom', 'age':100}
json_str = json.dumps(data)
print(json_str)

#output: {"name": "Tom", "age": 100} 注意：单引号被强制转化为双引号
```

写入文件：

```python
import json  #"json"全小写

data = {'name':'Tom', 'age':100}
with open('data.json', 'w', encoding='utf-8') as fw:
    json.dump(data, fw)
```

打开`data.json`文件可以看到内容：

```python
{"name": "Tom", "age": 100}
```

如需要格式化，可以加上参数`indent`：

```python
import json  #"json"全小写

data = {'name':'Tom', 'age':100}
with open('data.json', 'w', encoding='utf-8') as fw:
    json.dump(data, fw, indent=2)  #indent=2或indent=4
```

再打开`data.json`文件：

```python
{
  "name": "Tom",
  "age": 100
}
```

## 3. 字典dict和json的区别

- Python的字典dict是一种数据结构，json是一种数据传输格式。

  json就是一个根据某种约定格式编写的纯字符串，没有方法调用。编程语言无关，只负责传输数据。

  dict实现了一切自身该有的算法，可以直接调用方法进行操作，如dict.items()。

- dict中的字符串可用单引号或双引号，json中强制使用双引号。

- json的key必须是字符串，dict的key是hashable(可哈希的)。

  比如：{ (1,2) : 1} 在python里是合法的，因为tuple类型是hashable；而在json中是不合法的，只能是{ "(1,2)" : "1"}。

- json: true false null; python: True False None.

- json中的中文必须是unicode编码。

  python中{"me": "我"} 是合法的； json中必须是 {"me": "\u6211"}。

  