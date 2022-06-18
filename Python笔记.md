# Python笔记

## 解释器

命令行输入的参数可以从`sys`模块的`argv`变量里面获取。该列表至少有一个参数

## 基本语法

`range`函数返回的对象是一个列表，但是在迭代时，返回的是一个iterable对象。

```python
for i in range(1, 5):
    print("index" + i)
```

`pass`: 占位语句

`match`:

```python
match point:
    case 0 | 1 | 2:
        print("first")
    case (0, y):
        print("元组匹配")
    case _:
        print("else：通配符")
```

`match`守护项：

```python
match point:
    case Point(x, y) if x == y:
        print("满足守护项的条件才会进入")
    case Point(x, y):
        print("不满足守护项的条件")
```

`match`序列支持扩展包操作：`[x, y, *rest]` 和 `(x, y, *rest)` 的作用类似于解包赋值。支持通配符`(x, y, *_)`

### 函数

函数在 *执行* 时使用函数局部变量符号表，所有函数变量赋值都存在局部符号表中；引用变量时，首先，在局部符号表里查找变量，然后，是外层函数局部符号表，再是全局符号表，最后是内置名称符号表。

在调用函数时会将实际参数（实参）引入到被调用函数的局部符号表中；因此，实参是使用 *按值调用* 来传递的（其中的 *值* 始终是对象的 *引用* 而不是对象的值）

当一个函数调用另外一个函数时，会为该调用创建一个新的局部符号表。

函数定义在当前符号表中把函数名与函数对象关联在一起。函数名指向的对象可以用其他名称来指向。

函数默认值只计算一次，但是如果是对象或者集合类型，会发生改变，因为函数形参在符号表中是引用。

元组和字典形参：

```python
def cheeseshop(kind, *arguments, **keywords):
    # 元组循环
    for arg in arguments:
        print(arg)
    # 字典循环，默认取key
    for kw in keywords:
        print(kw, ":", keywords[kw])
    pass

cheeseshop("Limburger", 
           # 元组
           "It's very runny, sir.",
           "It's really very, VERY runny, sir.",
           # 字典
           shopkeeper="Michael Palin",
           client="John Cleese",
           sketch="Cheese Shop Sketch")
```

特殊参数：

```python
def f(pos1, pos2, /, pos_or_kwd, *, kwd1, kwd2):
      -----------    ----------     ----------
        |             |                  |
        |        Positional or keyword   |
        |                                - Keyword only
         -- Positional only
```

`/` 用于在逻辑上分割仅限位置形参与其它形参，如果函数定义中没有 `/`，则表示没有仅限位置形参。`/` 后可以是 *位置或关键字* 或 *仅限关键字* 形参。

把形参标记为 *仅限关键字*，表明必须以关键字参数形式传递该形参，应在参数列表中第一个 *仅限关键字* 形参前添加 `*`。

#### 解包实参列表

用*解包：

```python
args = [1, 7]
print(list(range(*args))) # [1, 2, 3, 4, 5, 6]
```

用**操作符传递关键字参数（解包字典）：

```python
def f(v, a='2', b='3'):
    print(v + a + b)

    
m = {'v': '1', 'a': '5', 'b': '10'}

f(**m) # 1510
```

#### Lambda表达式

格式`lambda a, b: a+b`

```python
def create_function(n):
    return lambda x: x + n


f = create_function(20)

print(f(1))
```

#### 文档字符串

```python
>>> def my_function():
...     """Do nothing, but document it.
...
...     No, really, it doesn't do anything.
...     """
...     pass
...
>>> print(my_function.__doc__)
Do nothing, but document it.

    No, really, it doesn't do anything.
```

### 数据结构

在列表开头插入或移除元素却很慢（因为所有其他元素都必须移动一位）。

实现队列最好用 `collections.deque`，可以快速从两端添加或删除元素

#### 列表推导式

```python
[(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
# 输出[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```

```python
[x**2 for x in range(10)] # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

第一个表达式是每次循环返回的结果，根据每次的结果生成列表。后面的for和if用来遍历输入范围。

嵌套列表推导式，先计算后面的for语句

```python
[[row[i] for row in matrix] for i in range(4)]
# [[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```

#### del语句

可以删除变量，也可以删除数组中的元素，可以删除数组中某个范围的元素

```python
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> del a
```

#### 元组

元组也是一种序列，列表和字符串也是序列。元组由多个用逗号隔开的值组成

```python
t = 12345, 54321, 'hello!'
t[0] # 12345
t # (12345, 54321, 'hello!')
```

输出时，元组都要由圆括号标注，这样才能正确地解释嵌套元组。输入时，圆括号可有可无。

元组是 [immutable](https://docs.python.org/zh-cn/3/glossary.html#term-immutable) （不可变的）。元组元素可以是异质，但是列表元素必须是同质。

```python
>>> empty = () # 空元组
>>> singleton = 'hello',  # 加逗号表示只有一个值的元组
x, y, z = t # 序列解包
```

#### 集合

> 集合是由不重复元素组成的无序容器

创建集合用花括号或 [`set()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#set) 函数。注意，创建空集合只能用 `set()`，不能用 `{}`，`{}` 创建的是空字典

```python
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)              
{'orange', 'banana', 'pear', 'apple'} # 重复的会被删除
```

集合推导式：

```python
a = {x for x in 'abracadabra' if x not in 'abc'}
# {'r', 'd'}
```

#### 字典

key通常是字符串或数字，也可以是其他任意不可变类型。

字典推导式：

```python
{x: x**2 for x in (2, 4, 6)}
# {2: 4, 4: 16, 6: 36}
```

#### 循环技巧

字典循环：用 `items()` 方法可同时取出键和对应的值

序列循环：用 [`enumerate()`](https://docs.python.org/zh-cn/3/library/functions.html#enumerate) 函数可以同时取出位置索引和对应的值

同时循环两个或多个序列时，用 [`zip()`](https://docs.python.org/zh-cn/3/library/functions.html#zip) 函数可以将其内的元素一一匹配

逆向循环序列时，先正向定位序列，然后调用 [`reversed()`](https://docs.python.org/zh-cn/3/library/functions.html#reversed) 函数

#### 深入条件控制

比较操作支持链式操作，例如，`a < b == c` 校验 `a` 是否小于 `b`，且 `b` 是否等于 `c`。

布尔运算符 `and` 和 `or` 也称为 *短路* 运算符。

在表达式内部赋值必须显式使用 [海象运算符](https://docs.python.org/zh-cn/3/faq/design.html#why-can-t-i-use-an-assignment-in-an-expression) `:=`

```python
while chunk := fp.read(200): # 3.8开始支持
   print(chunk)
```

### 模块

通过全局变量 `__name__` 可以获取模块名（即字符串）

模块有自己的私有符号表，用作模块中所有函数的全局符号表。因此，在模块内使用全局变量时，不用担心与用户定义的全局变量发生冲突。

被导入的模块名存在导入方模块的全局符号表里。

[`import`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#import) 语句有一个变体，可以直接把模块里的名称导入到另一个模块的符号表。

```python
from fibo import fib, fib2
import fibo as fib # 模块名后使用 as 时，直接把 as 后的名称与导入模块绑定。
from fibo import fib as fibonacci
```

#### 以脚本方式执行模块

```python
python fibo.py <arguments>
```

这项操作将执行模块里的代码，和导入模块一样，但会把 `__name__` 赋值为 `"__main__"`。 也就是把下列代码添加到模块末尾：

```python
if __name__ == "__main__":
    import sys
    fib(int(sys.argv[1]))
```

解析命令行的代码只有在模块以 “main” 文件执行时才会运行。

#### 模块搜索路径

当一个`spam.py`模块被import时，搜索路径为[`sys.builtin_module_names`](https://docs.python.org/zh-cn/3/library/sys.html#sys.builtin_module_names) -> 被[`sys.path`](https://docs.python.org/zh-cn/3/library/sys.html#sys.path). [`sys.path`](https://docs.python.org/zh-cn/3/library/sys.html#sys.path) 初始化的路径下的`spam.py`

#### 已编译的Python文件

为了快速加载模块，Python 把模块的编译版缓存在 `__pycache__` 目录中，文件名为 `module.version.pyc`，version 对编译文件格式进行编码，一般是 Python 的版本号。例如，CPython 的 3.3 发行版中，spam.py 的编译版本缓存为 `__pycache__/spam.cpython-33.pyc`。

#### dir()函数 - 列出模块内定义的名称列表

```python
>>> import fibo, sys
>>> dir(fibo) # 列出fibo模块定义的名称
['__name__', 'fib', 'fib2']
>>> dir() # 没有参数时，列出当前模块定义的名称
['__builtins__', '__name__', 'a', 'fib', 'fibo', 'sys']
```

#### 包

就是模块集合，一个包下面会有很多模块文件，Python 只把含 `__init__.py` 文件的目录当成包。

注意，相对导入基于当前模块名。因为主模块名是 `"__main__"` ，所以 Python 程序的主模块必须始终使用绝对导入。

### 类

如果把名称声明为全局变量，则所有引用和赋值将直接指向包含该模块的全局名称的中间作用域。重新绑定在最内层作用域以外找到的变量，使用 [`nonlocal`](https://docs.python.org/zh-cn/3/reference/simple_stmts.html#nonlocal) 语句把该变量声明为非局部变量。未声明为非局部变量的变量是只读的，（写入只读变量会在最内层作用域中创建一个 **新的** 局部变量，而同名的外部变量保持不变。）

#### 作用域

`global`声明修改全局作用域

`nonlocal`声明修改外层作用域





