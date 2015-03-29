## 数据结构
这一章节介绍你之前可能学习过的部分内容，同时添加一些新的内容。

### 5.1更多关于列表的内容
列表这种数据结构有很多方法，如下是一个列表对象的全部方法：

```
list.append(x)
```
在列表的末尾插入元素x,等同于 ```a[len(a):] = [x]```
```
list.extend(L)
```
将L中的全部元素插入到列表的末尾，以扩展该列表. 等同于```a[len(a):] = L.```
```
list.insert(i,x)
```
将一个元素插入到指定位置。第一个参数是待插入元素的前一个元素的位置，所以,``` a.insert(0,x)``` 在列表的最前边插入，而```a.insert(len(a),x)```等同于```a.append(x)```
```
list.remove(x)
```
从列表中移除第一个值为x的元素。如果没有这样的元素的话，该命令将会报错。
```
list.pop([i])
```
移除列表中给定位置的元素，并且返回它。如果未指定位置，```a.pop()```将移除并返回列表中的最后一个值。（方法中i周围的中括号表面该参数是可选的，而不是指你需要在这个位置输入一个中括号，在Python的库文档中，你会经常遇到这种表示）。
```
list.clear()
```
从列表中移除所有元素，等同于```del a[:]```
```
list.index(x)
```
返回列表中第一个值为i的元素的序号，如果没有这样的元素，该命令将会报错.
```
list.count(x)
```
返回列表中x出现的次数。
```
list.sort()
```
对该列表进行排序.
```
list.reverse()
```
翻转列表中的元素.
```
list.copy()
```
返回该列表的一个浅拷贝，等同于```a[:]```

一个使用了大多数列表中方法的例子
```
>>> a = [66.25, 333, 333, 1, 1234.5]
>>> print(a.count(333), a.count(66.25), a.count('x'))
2 1 0
>>> a.insert(2, -1)
>>> a.append(333)
>>> a
[66.25, 333, -1, 333, 1, 1234.5, 333]
>>> a.index(333)
1
>>> a.remove(333)
>>> a
[66.25, -1, 333, 1, 1234.5, 333]
>>> a.reverse()
>>> a
[333, 1234.5, 1, 333, -1, 66.25]
>>> a.sort()
>>> a
[-1, 1, 66.25, 333, 333, 1234.5]
>>> a.pop()
1234.5
>>> a
[-1, 1, 66.25, 333, 333]
```
你可能已经注意到，诸如```insert```,```remove```,```sort```这样仅仅修改列表的方法并没有打印出返回值，它们返回一个默认的```None```.这是Python中所有可变的数据结构的一个设计准则。

#### 5.1.1 将列表作为栈来使用
列表的方法使得将列表作为栈来使用是非常容易的,栈是一种后进入的元素最先被取出(last-in, first-out)的数据结构。要将一个元素加入栈顶，使用```append()```方法，要将栈顶元素取出，使用无特定序号的```pop()```方法。例如
```
>>> stack = [3, 4, 5]
>>> stack.append(6)
>>> stack.append(7)
>>> stack
[3, 4, 5, 6, 7]
>>> stack.pop()
7
>>> stack
[3, 4, 5, 6]
>>> stack.pop()
6
>>> stack.pop()
5
>>> stack
[3, 4]
```

#### 5.1.2 将列表作为队列来使用
将列表用作队列同样是可以的，队列是一种先进入的元素被先取出(first-in,first-out)的数据结构。然而，列表并不足以满足这一要求，因为在列表的末尾添加和弹出数据都是（相对）快的，但是在列表的头部插入或弹出却（比较）慢（因为其他的元素要被一个个地移动）。
要实现一个队列，可以使用```collections.deque```，它被设计用来实现从两端都可以快速添加和弹出的方法。例如
```
>>> from collections import deque
>>> queue = deque(["Eric", "John", "Michael"])
>>> queue.append("Terry")           # Terry arrives
>>> queue.append("Graham")          # Graham arrives
>>> queue.popleft()                 # The first to arrive now leaves
'Eric'
>>> queue.popleft()                 # The second to arrive now leaves
'John'
>>> queue                           # Remaining queue in order of arrival
deque(['Michael', 'Terry', 'Graham'])
```

#### 5.1.3列表推导式
列表推导式提供了一个创建列表的具体的方式，大多数应用程序建立的列表，列表中元素是另一个序列或可遍历的数据结构中元素的运算结果，或是满足特定条件的元素的子序列。
例如，假定我们需要建立一个平方数的列表，像这样：
```
>>> squares = []
>>> for x in range(10):
...     squares.append(x**2)
...
>>> squares
[0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```
注意，这将创建（或覆写）一个名为x的变量，并且这一变量在循环结束后仍将存在。而用下述的方式，我们能够得到一个没有任何副作用的平方数的列表。
```
squares = list(map(lambda x: x**2, range(10)))
```
或者，等价地：
```
squares = [x**2 for x in range(10)]
```
后者更加精确且更具有可读性。

一个列表推导式由一个包含了表达式，紧跟着```for```语句的括号，零个或更多的```for```或者```if```从句构成。表达式的结果将会从表达式中```for```语句和```if```语句运算得出。例如，下述的列表推导式结合了两个列表中不相等的元素。
```
>>> [(x, y) for x in [1,2,3] for y in [3,1,4] if x != y]
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```
这也等价于
```
>>> combs = []
>>> for x in [1,2,3]:
...     for y in [3,1,4]:
...         if x != y:
...             combs.append((x, y))
...
>>> combs
[(1, 3), (1, 4), (2, 3), (2, 1), (2, 4), (3, 1), (3, 4)]
```
注意，这两个片段中```for```和```if```的顺序是一样的。

如果表达式是一个元组(例如，前例中的```(x,y)```)，它(元组中的数据)必须加上括号。
```
>>> vec = [-4, -2, 0, 2, 4]
>>> # create a new list with the values doubled
>>> [x*2 for x in vec]
[-8, -4, 0, 4, 8]
>>> # filter the list to exclude negative numbers
>>> [x for x in vec if x >= 0]
[0, 2, 4]
>>> # apply a function to all the elements
>>> [abs(x) for x in vec]
[4, 2, 0, 2, 4]
>>> # call a method on each element
>>> freshfruit = ['  banana', '  loganberry ', 'passion fruit  ']
>>> [weapon.strip() for weapon in freshfruit]
['banana', 'loganberry', 'passion fruit']
>>> # create a list of 2-tuples like (number, square)
>>> [(x, x**2) for x in range(6)]
[(0, 0), (1, 1), (2, 4), (3, 9), (4, 16), (5, 25)]
>>> # the tuple must be parenthesized, otherwise an error is raised
>>> [x, x**2 for x in range(6)]
  File "<stdin>", line 1, in ?
    [x, x**2 for x in range(6)]
               ^
SyntaxError: invalid syntax
>>> # flatten a list using a listcomp with two 'for'
>>> vec = [[1,2,3], [4,5,6], [7,8,9]]
>>> [num for elem in vec for num in elem]
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```
列表推导式可以包含复杂的表达式和嵌套的函数，例如：
```
>>> from math import pi
>>> [str(round(pi, i)) for i in range(1, 6)]
['3.1', '3.14', '3.142', '3.1416', '3.14159']
```

#### 5.1.4 嵌套列表推导式
一个列表推导式中最初的表达式可以是任何表达式，包括另一个列表推导式。
下面的一个3*4的矩阵由一个包含3个长度为4的列表的的列表实现。
```
>>> matrix = [
...     [1, 2, 3, 4],
...     [5, 6, 7, 8],
...     [9, 10, 11, 12],
... ]
```
下面的列表推导式将会（对该矩阵）进行行和列的转置：
```
>>> [[row[i] for row in matrix] for i in range(4)]
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```
正如我们在前一节所看到的，嵌套的列表推导式由紧跟着上下文的```for```语句得出，所以，这个例子等价于
```
>>> transposed = []
>>> for i in range(4):
...     transposed.append([row[i] for row in matrix])
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```
相应地，也等价于：
```
>>> transposed = []
>>> for i in range(4):
...     # the following 3 lines implement the nested listcomp
...     transposed_row = []
...     for row in matrix:
...         transposed_row.append(row[i])
...     transposed.append(transposed_row)
...
>>> transposed
[[1, 5, 9], [2, 6, 10], [3, 7, 11], [4, 8, 12]]
```
在实际编程时，相比较于复杂的控制流语句，我们可能更喜欢内建的函数。对于这种情况，```zip()```方法可以做的很好：
```
>>> list(zip(*matrix))
[(1, 5, 9), (2, 6, 10), (3, 7, 11), (4, 8, 12)]
```
关于这个例子中*的含义，你可以参见<I>解压参数列表</I> 一节

### 5.2 ```del```语句
我们可以使用序号而不是它的值来从一个列表中移除一项，有一种方式是使用```del```语句。这同要返回一个值的```pop()```方法不同。```del```语句也可以被用于从列表中移除切片或者整个列表(之前我们是把一个空链表赋给切片)，例如：
```
>>> a = [-1, 1, 66.25, 333, 333, 1234.5]
>>> del a[0]
>>> a
[1, 66.25, 333, 333, 1234.5]
>>> del a[2:4]
>>> a
[1, 66.25, 1234.5]
>>> del a[:]
>>> a
[]
```
```del```语句也可被用于删除整个变量：
```
>>> del a
```
在此之后再去使用a将会导致错误（至少在另一个值被赋给a之前）。以后我们将介绍更多```del```语句的用法。
