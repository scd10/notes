# Python3学习笔记

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

## 迭代器
- 集合类型`list`,`dict`, `str`都是`Iterable`，而不是`Iterator`。
- `generator`都是`Iterator`。
- `list`, `dict`, `str`都可以用`iter()`函数转换为`Iterator`。
- 可用于`for`循环的都是`Iterable`。
- 可用于`next()`函数的都是`Iterator`。

## 高阶函数

### map/reduce
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

### filter
- Python内建的`filter()`函数用于过滤序列。和`map()`类似，`filter()`
也接收一个函数和一个序列，并把函数作用于序列的每一个元素，根据返回值是
`True`还是`False`决定是保留还是丢弃该元素。

例如在一个list里保留奇数，删掉偶数：
```
def is_odd(n):
  return n % 2 == 1
list(filter(is_odd, [1,2,3,4,5,6,7,8,9]))
```

### sorted
- `sorted()`可以对list进行排序，同时`sorted`也是一个高阶函数，可以接受
一个`key`函数参数来自定义排序，例如:

```
sorted([1,4,6,-2,-7], abs)
sorted(['bob', 'about', 'Zoo', 'Credit'], key=str.lower)
```

要反向排序，只需要再传入第三个参数`reverse=True`

## 返回函数
**高阶函数除了可以接受函数作为参数外，还可以把函数作为结果返回。**