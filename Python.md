# Python3学习笔记

[廖雪峰的Python教程](https://www.liaoxuefeng.com/)

## 1. 列表生成
`[x * x for x in range(1,11)]`
用来替代循环

## 2. 生成器 generator
` g=(x * x for x in range(1,11)) `
用`[]`和`()`就能区别生成list还是generator。

```
def fib(max):
  n, a, b = 0, 0, 1
  while n < max:
    yield b
    a, b = b, a+b
    n += 1
  return
```

生成过程中`next()`到`yield`语句中断，下一个`next()`语句再继续执行。

```
try:
  next(g)
  ...
except StopIteration as e:
  ...
```

`next()`需要设置退出条件。

## 3. 迭代器
- 集合类型`list`,`dict`, `str`都是`Iterable`，而不是`Iterator`。
- `generator`都是`Iterator`。
- `list`, `dict`, `str`都可以用`iter()`函数转换为`Iterator`。
- 可用于`for`循环的都是`Iterable`。
- 可用于`next()`函数的都是`Iterator`。

## 4. 高阶函数

### 4.1 map()/reduce()
- `map`函数接收2个参数，第一个是函数，第二个是`Iterable`，`map`
将传入的函数依次作用到序列的每一个元素，并把结果作为新的`Iterator`
返回

```
a = list(map(lambda x: x*x, [1,2]))
```

比写循环的好处是比较含义更为直观。

- `reduce`是把结果继续和序列的下一个元素做累积计算，效果就是：

```
reduce(f,[x1, x2, x3, x4]) = f(f(f(x1, x2), x3), x4)
```

### 4.2 filter()
- Python内建的`filter()`函数用于过滤序列。和`map()`类似，`filter()`
也接收一个函数和一个序列，并把函数作用于序列的每一个元素，根据返回值是
`True`还是`False`决定是保留还是丢弃该元素。

例如在一个list里保留奇数，删掉偶数：
```
def is_odd(n):
  return n % 2 == 1
list(filter(is_odd, [1,2,3,4,5,6,7,8,9]))
```

### 4.3 sorted()
- `sorted()`可以对list进行排序，同时`sorted`也是一个高阶函数，可以接受
一个`key`函数参数来自定义排序，例如:

```
sorted([1,4,6,-2,-7], abs)
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
```

要反向排序，只需要再传入第三个参数`reverse=True`

## 5. 返回函数
**高阶函数除了可以接受函数作为参数外，还可以把函数作为结果返回。**

## 6. 匿名函数
- `lambda x: x*x`，`lambda`表示这是个匿名函数，`x`为函数参数。
- 匿名函数有个限制，只能有一个表达式，不用写`return`

## 7. 装饰器
**to be finished.**

## 8. Partial Function
- `functools.partial()`
```
>>> import functools
>>> int2 = functools.partial(int, base=2)
>>> int2('1000000')
64
```
- 作用就是把一个函数的某些参数给固定住(也就是设定默认值)，返回一个新的
函数，调用新函数更方便一些。

## 9. 面向对象编程
### 9.1. 类和实例
- `__init__(self, x)`函数定义后，初始化类的实例必须按照函数的参数进行初始化。
- 要让类内部的属性不被外部访问，可以在属性的名称前加两个下划线`__`，设为
私有变量
- 参数检查：防止无效参数传入
- 判断对象类型
  - `type(a)`: 返回一个对象的类型
  - `instance('a', str)`: 判断一个实例是否是某一个类型
  - `dir()`: 获得一个对象的所有属性和方法
  - 类似`__xxx__`的方法有特殊用途，例如：`len('abc')`实际上等效于
  `abc'.__len__()`
  - `@property`广泛应用在类的定义中，可以让调用者写出简短的代码，同时保证对参数进行必
  要的检查，这样，程序运行时就减少了出错的可能性。

```
class Student(object):

    @property
    def score(self):
        return self._score

    @score.setter
    def score(self, value):
        if not isinstance(value, int):
            raise ValueError('score must be an integer!')
        if value < 0 or value > 100:
            raise ValueError('score must between 0 ~ 100!')
        self._score = value

    @property
    def birth(self):                      # 读
        return self._birth

    @birth.setter
    def birth(self, value):               # 写
        self._birth = value

    @property
    def age(self):                        # 只读
        return 2015 - self._birth

>>> s = Student()
>>> s.score = 60     # OK，实际转化为s.set_score(60)
>>> s.score          # OK，实际转化为s.get_score()
60
>>> s.score = 9999
Traceback (most recent call last):
  ...
ValueError: score must between 0 ~ 100!
```

- `__str__()`, `__repr__()`
```
class Student(object):
  def __init__(self, name):
    self.name = name
  def __str__(self):
    return 'Student object (name=%s)' % self.name
  __repr__ = __str__
```

- `__getitem__()`：像list一样按下标取元素
```
class Fib(object):
  def __getitem__(self, n):
    a, b = 0, 1
    for x in range(n):
      a, b = b, a + b
    return a
```

- `__getattr__()`: 定义类属性相关的内容，处理`AttributeError`错误。
- `__call__()`: 直接调用对象， 把对象看成函数

### 9.2 枚举类
```
from enum import Enum

Month = Enum('Month',('Jan', 'Feb', ...))
```

### 9.3 元类
- 动态创建类

## 10. 调试
- `print()`
- `assert`
- `logging`
```
import logging
logging.basicConfig(level = logging.INFO)
```
- `pdb`，'l'用来看代码，'n'来单步执行，'p 变量名'查看变量，'q'结束，
`pdb.set_trace()`设置断点
```
$ python -m pdb err.py
```

## 11. IO
### 11.1 文件读写
- 文件打开后一般需要调用`f.close()`关闭，因为打开的文件会占用操作系统资源。
为防止错误，最好使用`try... except...`来捕获`IOError`，但可以用另一种方法，
`with`语句，代码更简洁，不用调用`f.close()`方法。
```
with open('/path/to/file','r') as f:
  print(f.read())
```
- 调用`read()`会把所有文件一次性读入内存，也可以用`read(size)`分批读入

### 11.2 file-like Object
- 像`open()`函数返回的这种有个`read()`方法的对象,在Python中统称为file-like Object

### 11.3 字符编码
遇到有些编码不规范的文件，你可能会遇到`UnicodeDecodeError`，因为在文本文件中可能夹杂
了一些非法编码的字符。遇到这种情况，`open()`函数还接收一个`errors`参数，表示如果遇到
编码错误后如何处理。最简单的方式是直接忽略：
```
f = open('/Users/michael/gbk.txt', 'r', encoding='gbk', errors='ignore')
```

### 11.4 StringIO和BytesIO
- 在内存中读写str和二进制

### 11.5 序列化
- `pickle`
```
>>> f = open('dump.txt', 'wb')
>>> pickle.dump(d, f)
>>> f.close()

>>> f = open('dump.txt', 'rb')
>>> d = pickle.load(f)
>>> f.close()
```

- JSON

如果我们要在不同的编程语言之间传递对象，就必须把对象序列化为标准格式，比如XML，
但更好的方法是序列化为JSON，因为JSON表示出来就是一个字符串，可以被所有语言读取，
也可以方便地存储到磁盘或者通过网络传输。JSON不仅是标准格式，并且比XML更快，
而且可以直接在Web页面中读取，非常方便。

Python内置的json模块提供了非常完善的Python对象到JSON格式的转换。
```
>>> import json
>>> d = dict(name='Bob', age=20, score=88)
>>> json.dumps(d)
'{"age": 20, "score": 88, "name": "Bob"}'
```

## 12. 进程和线程
- Linux和Mac系统可以直接调用`os.fork()`启动进程，windows没有这个功能。
- `multiprocessing`的`Process`可以实现跨平台的多进程。
```
from multiprocessing import Process
import os

# 子进程要执行的代码
def run_proc(name):
    print('Run child process %s (%s)...' % (name, os.getpid()))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Process(target=run_proc, args=('test',))
    print('Child process will start.')
    p.start()
    p.join()
    print('Child process end.')
```
创建子进程时，只需要传入一个执行函数和函数的参数，创建一个`Process`实例，用`start()`
方法启动。`join()`方法可以等待子进程结束后再继续往下运行，通常用于**进程间同步**。

- **Pool** 如果要启动大量的子进程，可以用进程池。
```
from multiprocessing import Pool
import os, time, random

def long_time_task(name):
    print('Run task %s (%s)...' % (name, os.getpid()))
    start = time.time()
    time.sleep(random.random() * 3)
    end = time.time()
    print('Task %s runs %0.2f seconds.' % (name, (end - start)))

if __name__=='__main__':
    print('Parent process %s.' % os.getpid())
    p = Pool(4)
    for i in range(5):
        p.apply_async(long_time_task, args=(i,))
    print('Waiting for all subprocesses done...')
    p.close()
    p.join()
    print('All subprocesses done.')
```
对Pool对象调用`join()`方法会等待所有子进程执行完毕，调用`join()`之前必须先调用
`close()`，调用`close()`之后就不能继续添加新的进程了。Pool的默认大小是CPU的核数。

- **进程间通信** Python的`multiprocessing`模块包装了底层机制，提供了`Queue`、`Pipes`
等多种方式来交换数据。

- **多线程** 多线程和多进程最大的不同在于，多进程中，同一个变量，各自有一份拷贝存在于
每个进程中，互不影响，而多线程中，所有变量都由所有线程共享，所以，任何一个变量都可以被
任何一个线程修改，因此，线程之间共享数据最大的危险在于多个线程同时改一个变量，
把内容给改乱了。Python中处理这一问题的方法是`threading.Lock()`:
```
balance = 0
lock = threading.Lock()

def run_thread(n):
    for i in range(100000):
        # 先要获取锁:
        lock.acquire()
        try:
            # 放心地改吧:
            change_it(n)
        finally:
            # 改完了一定要释放锁:
            lock.release()
```
多线程在Python中只能交替执行，即使100个线程跑在100核CPU上，也只能用到1个核。

- **分布式进程** `multiprocessing.managers`支持把多进程分布到多台机器上。

## 13. 常用内建模块
### 13.1 collections
- **namedtuple**
```
>>> from collections import namedtuple
>>> Point = namedtuple('Point', ['x', 'y'])
>>> p = Point(1, 2)
>>> p.x
1
>>> p.y
2
```
`namedtuple`是一个函数，它用来创建一个自定义的`tuple`对象，并且规定了tuple元素的个数，
并可以用属性而不是索引来引用tuple的某个元素。`namedtuple`很方便地定义一种数据类型，
它具备tuple的不变性，又可以根据属性来引用。

- **deque**

`list`存储数据时，按索引访问元素很快，但是插入和删除元素很慢。

`deque`是为了高效实现插入和删除的双向列表，适用于队列和栈。`deque`除了实现`list`的
`append()`和`pop()`外，还支持`appendleft()`和`popleft()`，这样就可以非常高效地往
头部添加或删除元素。
```
>>> from collections import deque
>>> q = deque(['a', 'b', 'c'])
>>> q.append('x')
>>> q.appendleft('y')
>>> q
deque(['y', 'a', 'b', 'c', 'x'])
```

- **defaultdict**

使用dict时，如果引用的Key不存在，就会抛出KeyError。如果希望key不存在时，
返回一个默认值，就可以用`defaultdict`。除了在Key不存在时返回默认值，
defaultdict的其他行为跟dict是完全一样的。

- **OrderedDict**

使用`dict`时，Key是无序的。在对`dict`做迭代时，我们无法确定Key的顺序。如果要保持Key
的顺序，可以用`OrderedDict`。
```
>>> from collections import OrderedDict
>>> d = dict([('a', 1), ('b', 2), ('c', 3)])
>>> d # dict的Key是无序的
{'a': 1, 'c': 3, 'b': 2}
>>> od = OrderedDict([('a', 1), ('b', 2), ('c', 3)])
>>> od # OrderedDict的Key是有序的
OrderedDict([('a', 1), ('b', 2), ('c', 3)])
```
**注意**，`OrderedDict`的Key会按照插入的顺序排列，不是Key本身排序。

- **Counter**

```
>>> from collections import Counter
>>> c = Counter()
>>> for ch in 'programming':
...     c[ch] = c[ch] + 1
...
>>> c
Counter({'g': 2, 'm': 2, 'r': 2, 'a': 1, 'i': 1, 'o': 1, 'n': 1, 'p': 1})
```

### 13.2 base64
Base64是一种任意二进制到文本字符串的编码方法，常用于在URL、Cookie、
网页中传输少量二进制数据。

### 13.3 struct
准确地讲，Python没有专门处理字节的数据类型。好在Python提供了一个struct模块来解决
bytes和其他二进制数据类型的转换。

### 13.4 hashlib
- MD5: `hashilib.md5()`, SHA1: `hashlib.sha1()`
```
import hashlib

md5 = hashlib.md5()
md5.update('how to use md5 in '.encode('utf-8'))
md5.update('python hashlib?'.encode('utf-8'))
print(md5.hexdigest())
```

### 13.5 itertools
- `count()`
- `cycle()`; `cs = itertools.cycle('ABC')`
- `repeat()`; `ns = itertools.repeat('A', 3)`
- 无限序列只有在for迭代时才会无限地迭代下去，如果只是创建了一个迭代对象，它不会事先把
无限个元素生成出来，事实上也不可能在内存中创建无限多个元素。无限序列虽然可以无限迭代
下去，但是通常我们会通过takewhile()等函数根据条件判断来截取出一个有限的序列
```
>>> natuals = itertools.count(1)
>>> ns = itertools.takewhile(lambda x: x <= 10, natuals)
>>> list(ns)
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
```
- `chain()` 可以把一组迭代对象串联起来，形成一个更大的迭代器。
```
>>> for c in itertools.chain('ABC', 'XYZ'):
...     print(c)
# 迭代效果：'A' 'B' 'C' 'X' 'Y' 'Z'
```
- `groupby()`把迭代器中相邻的重复元素挑出来放在一起。
```
>>> for key, group in itertools.groupby('AAABBBCCAAA'):
...     print(key, list(group))
...
A ['A', 'A', 'A']
B ['B', 'B', 'B']
C ['C', 'C']
A ['A', 'A', 'A']
```

### 13.6 contextlib
- `with`语句
- 定义`__enter__`和`__exit__`实现上下文管理。
- `@contextmanager`
- `@closing`

### 13.7 urllib
### 13.8 XML
### 13.9 HTMLParser

## 14. 网络编程
## 15. 电子邮件
## 16. 数据库
## 17. Web开发
## 18. 异步IO
### 18.1 协程
### 18.2 asyncio
### 18.3 async/await
### 18.4 aiohttp
