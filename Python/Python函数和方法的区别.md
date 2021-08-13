# Python函数和方法的区别

<p align="right">2021年8月，时间结余。</p>

---

函数(function)，方法(method)。

先上代码：

```python
def cut():
    pass

print('cut:', cut)
print('='*79)

class Apple:
    def look(self):
        pass

    @staticmethod
    def smell():
        pass

    @classmethod
    def eat(cls):
        pass

print('Apple.look:', Apple.look)
print('Apple.smell:', Apple.smell)
print('Apple.eat:', Apple.eat)

print('='*79)

apple = Apple()
print('apple.look:', apple.look)
print('apple.smell:', apple.smell)
print('apple.eat:', apple.eat)
```

输出结果:

```python
cut: <function cut at 0x000002826DCFC280>
===============================================================================
Apple.look: <function Apple.look at 0x000002826DCFC4C0>
Apple.smell: <function Apple.smell at 0x000002826DCFC550>
Apple.eat: <bound method Apple.eat of <class '__main__.Apple'>>
===============================================================================
apple.look: <bound method Apple.look of <__main__.Apple object at 0x000002826DCFB310>>
apple.smell: <function Apple.smell at 0x000002826DCFC550>
apple.eat: <bound method Apple.eat of <class '__main__.Apple'>>
```

翻译一下:

| 名称            | 类型 |
| --------------- | ---- |
| `cut()`         | 函数 |
| `Apple.look()`  | 函数 |
| `Apple.smell()` | 函数 |
| `Apple.eat()`   | 方法 |
| `apple.look()`  | 方法 |
| `apple.smell()` | 函数 |
| `apple.eat()`   | 方法 |

可以看出:

- 普通的function是一个函数。
- 在class内定义的普通方法，如`look()`，是面向实例化对象的，是实例的方法。它属于方法。
- 在class内定义的静态方法，如`smell()`，它与任何对象都没有联系，等同于是在class外定义的function，它属于函数。
- 在class内定义的类方法，如`eat()`，它第一个参数必须是`cls`，它与class本身是绑定关系，它属于方法。

总结：

1. 与类和实例无绑定关系的function都属于函数（function）。
2. 与类和实例有绑定关系的function都属于方法（method）。
