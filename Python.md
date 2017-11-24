# Python3学习笔记

## 列表生成
`[x * x for x in range(1,11)]`
用来替代循环

## 生成器 generator
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

