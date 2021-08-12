# 一文解决函数参数的问题

<p align="right">2021年8月，时间结余。</p>

---

## 1. json模块源码
以json源码为例。[cpython/__init__.py at 3.9 · python/cpython (github.com)](https://github.com/python/cpython/blob/3.9/Lib/json/__init__.py)

其中`load()`函数的定义是这样的：

```python
def load(fp, *, cls=None, object_hook=None, parse_float=None,
        parse_int=None, parse_constant=None, object_pairs_hook=None, **kw):
    return loads(fp.read(),
        cls=cls, object_hook=object_hook,
        parse_float=parse_float, parse_int=parse_int,
        parse_constant=parse_constant, object_pairs_hook=object_pairs_hook, **kw)
```

函数实现就是一行，调用`loads()`方法。

但是，我们发现函数定义里参数众多，有4种类型的：fp，*，cls=None，**kw

下面我们就看看这几种参数的意义。

---

## 2. 位置参数、关键字参数、默认可省略参数、可变不定长参数、*args、**kwargs
