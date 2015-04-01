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
你可能已经注意到，诸如```insert```,```remove```,```sort```这样仅仅修改列表的方法并没有打印出返回值，它们返回一个默认的```None```[1].这是Python中所有可变的数据结构的一个设计准则。

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

del语句也可被用于删除整个变量：

```
>>> del a
```
在此之后再去使用a将会导致错误（至少在另一个值被赋给a之前）。以后我们将介绍更多```del```语句的用法。

### 5.3 元组和序列
我们发现列表和字符串有着很多共同点，例如序列化和切片操作。他们是序列型数据类型的两个例子（参见<I>序列类型：列表，元组和范围</I>），因为Python是一门进化中的语言，其他的序列型数据类型可能会被添加进来。还有另外一种标准的序列化数据类型——元组
元组由一些用逗号分隔开的值组成，例如：
```
>>> t = 12345, 54321, 'hello!'
>>> t[0]
12345
>>> t
(12345, 54321, 'hello!')
>>> # Tuples may be nested:
... u = t, (1, 2, 3, 4, 5)
>>> u
((12345, 54321, 'hello!'), (1, 2, 3, 4, 5))
>>> # Tuples are immutable:
... t[0] = 88888
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'tuple' object does not support item assignment
>>> # but they can contain mutable objects:
... v = ([1, 2, 3], [3, 2, 1])
>>> v
([1, 2, 3], [3, 2, 1])
```
正如你所看到的，元组的输出总是用括号包围起来的，所以嵌套的元组也能够正确的解释。尽管括号经常是必要的（如果元组是一个更大的表达式的一部分），但元组的输入可以包含或者不包含括号。你不能够给单独的元组元素赋值，然而，你可以建立一个包含可变元素，例如列表的元组。

尽管元组看起来和列表很像，但他们还是经常用于不同的情况和不同的目的。元组是不可变的（即定长。——译者注），并且经常包含多种不同的通过解包（稍后介绍）或者索引（甚至于如果是```namedtuples```，可以通过属性来访问）得到的元素序列。列表是可变的（即定长），而且列表的元素通常通过列表的遍历来完成。

一个特殊的问题是，包含0个或1个元素的元组的构造：Python有一些特殊的语法来处理这种情况。空的元组可以通过一对空的括号来构造，有一个元素的元组可以通过在值后边跟随一个逗号来构造。（仅仅在一个单独的值外边加括号是不够的）。不好看，但有效。例如：
```
>>> empty = ()
>>> singleton = 'hello',    # <-- note trailing comma
>>> len(empty)
0
>>> len(singleton)
1
>>> singleton
('hello',)
```
表达式```t = 12345, 54321, 'hello!'```是一个元组解包的例子：值12345,,54321和"hello"打包在一个元组中。反过来的操作也是可以的。
```
>>> x, y, z = t
```
足够恰当地说，对所有右边的序列来言（如上例中的t），这就是所谓的序列解包。元素解包时，在等式的左边需要有和右边序列中元素数目同样多的变量。注意：多重赋值仅仅是元组打包和序列解包的结合。

### 5.4 集合
Python同样包含一种称为集合的数据类型。集合是一种无序的、不重复的数据结合。简单的使用包括测试元素是否在集合中和消除重复的元素。集合的实例同样支持诸如并、交、补以及对称差等操作。一个简单的例子：
```
>>> basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
>>> print(basket)                      # show that duplicates have been removed
{'orange', 'banana', 'pear', 'apple'}
>>> 'orange' in basket                 # fast membership testing
True
>>> 'crabgrass' in basket
False

>>> # Demonstrate set operations on unique letters from two words
...
>>> a = set('abracadabra')
>>> b = set('alacazam')
>>> a                                  # unique letters in a
{'a', 'r', 'b', 'c', 'd'}
>>> a - b                              # letters in a but not in b
{'r', 'd', 'b'}
>>> a | b                              # letters in either a or b
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
>>> a & b                              # letters in both a and b
{'a', 'c'}
>>> a ^ b                              # letters in a or b but not both
{'r', 'd', 'b', 'm', 'z', 'l'}
```
类似于列表推导式，Python也支持集合的推导式。
```
>>> a = {x for x in 'abracadabra' if x not in 'abc'}
>>> a
{'r', 'd'}
```
### 5.5 字典
Python中另一种有用的数据类型是字典（参见：映射类型——字典）。在其他的语言中，字典有时被称之为关联存储或者关联数组。与通过一组数来索引的序列不同，字典通过可以为任意常量的<B>键</B>来索引，例如字符串或者是数字。如果元组仅仅包含字符串、数字或者元组的话，那么元组也可以被用作为键。然而如果元组直接或间接地包含任何变量，那它就不能被用作键。因为列表能够通过索引赋值、切片赋值或者像```append()```或```extend()```这样的方法来改变，因而列表不能够被当做键。

最好的方式是，将字典视作一组要求键唯一（在同一字典内）的无序键值对的集合，一对大括号可以创建一个空字典```{}```.通过放置用逗号分隔开的键值对在括号里边可以向字典增加初始的键值对，这也是字典被写入和读取的方式。

对字典的主要的操作是存放键值对并通过键提取出值。通过```del```语句也可以删除一个键值对。如果你通过一个已经存在的键来存放值，和这个键相关联的旧的值将会被抹掉。通过一个不存在的键来读取值也是不被允许的。

通过在字典上的```list(d.keys())```操作可以返回在字典中用到的所有键，以一个任意的顺序（如果需要排序的话，可以使用```sorted(d.keys())```来代替[2]）。如果要检查一个单独的键是否在字典中，可以使用```in```关键字。

一个使用字典的简单例子：
```
>>> tel = {'jack': 4098, 'sape': 4139}
>>> tel['guido'] = 4127
>>> tel
{'sape': 4139, 'guido': 4127, 'jack': 4098}
>>> tel['jack']
4098
>>> del tel['sape']
>>> tel['irv'] = 4127
>>> tel
{'guido': 4127, 'irv': 4127, 'jack': 4098}
>>> list(tel.keys())
['irv', 'guido', 'jack']
>>> sorted(tel.keys())
['guido', 'irv', 'jack']
>>> 'guido' in tel
True
>>> 'jack' not in tel
False
```

使用```dict()```构造器直接从一个键值对序列建立字典：
```
>>> dict([('sape', 4139), ('guido', 4127), ('jack', 4098)])
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```
另外，如果要从任意的键值表达式中建立字典，可以使用字典推导式：
```
>>> {x: x**2 for x in (2, 4, 6)}
{2: 4, 4: 16, 6: 36}
```

当键是简单的字符串，使用关键字参数来确定键值对有时更加简单：
```
>>> dict(sape=4139, guido=4127, jack=4098)
{'sape': 4139, 'jack': 4098, 'guido': 4127}
```

### 5.6 遍历的技巧
当遍历一个字典时，键和与之对应的值可以通过```items()```方法同时得到。
```
>>> knights = {'gallahad': 'the pure', 'robin': 'the brave'}
>>> for k, v in knights.items():
...     print(k, v)
...
gallahad the pure
robin the brave
```
当遍历一个序列时，位置索引和与之对应的值可以通过```enumerate()```方法同时得到。
```
>>> for i, v in enumerate(['tic', 'tac', 'toe']):
...     print(i, v)
...
0 tic
1 tac
2 toe
```
如果要同时遍历两个或更多的序列，可以通过```zip()```方法来配对。
```
>>> questions = ['name', 'quest', 'favorite color']
>>> answers = ['lancelot', 'the holy grail', 'blue']
>>> for q, a in zip(questions, answers):
...     print('What is your {0}?  It is {1}.'.format(q, a))
...
What is your name?  It is lancelot.
What is your quest?  It is the holy grail.
What is your favorite color?  It is blue.
```
如果要翻转的遍历一个序列，首先确定序列的正方向，然后使用```reversed()```方法。
```
>>> for i in reversed(range(1, 10, 2)):
...     print(i)
...
9
7
5
3
1
```
要以一个排序过的顺序遍历一个序列，使用```sorted()```方法。该方法返回一个新的列表，同时保持原列表不变。
```
>>> basket = ['apple', 'orange', 'apple', 'pear', 'orange', 'banana']
>>> for f in sorted(set(basket)):
...     print(f)
...
apple
banana
orange
pear
```
如果要在循环内改变一个正在遍历的序列（例如赋值特定的元素），最好是先复制一份。遍历序列并不隐式地复制。切片表示让这非常的简单。
```
>>> words = ['cat', 'window', 'defenestrate']
>>> for w in words[:]:  # Loop over a slice copy of the entire list.
...     if len(w) > 6:
...         words.insert(0, w)
...
>>> words
['defenestrate', 'cat', 'window', 'defenestrate']
```
### 5.7 关于条件控制的更多内容
用于```while```和```if```语句的条件控制可以包含任何的操作符，不仅仅是比较。

比较操作符```in```和```not in```可以用于检查一个值是否在一个序列中出现。 ```is```和```not is``` 操作符可以比较两个实例是否为同一个实例，当然这仅仅对于如列表这样的可变的实例有作用。所有的比较操作符都有着相同的优先级，即比所有的数字运算符都要低。

比较符可以连接在一起。例如```a < b == c```可以测试a是否小于b，同时b等于c。

比较符可以和逻辑运算符结合在一起，例如```and```和```or```。而且这个比较符的输出（或者任意的逻辑表达式）可以通过```not```来取反。这些逻辑运算符的优先级都要低于比较操作符。而在他们中间，```not```有最高的优先级而```or```有最低的优先级，所以，```A and not B or C```和```(A and (not B)) or C```是等价的。因为括号总是可以表达所需要的组合。

逻辑表达式```and```和```or```被称为短路操作符：他们的参数从左到右被传入，并且当得到结果时立即就停止。例如，如果A和C为真而B为假，```A and B and C```并不求C表达式的值，如果他们被用于普通的数据，而不是作为逻辑值时，返回的结果就是最后的一个被求解的参数。

可以将比较结果或者逻辑表达式的值赋给一个变量，例如
```
>>> string1, string2, string3 = '', 'Trondheim', 'Hammer Dance'
>>> non_null = string1 or string2 or string3
>>> non_null
'Trondheim'
```
注意：在Python中，和C不同的是，赋值操作不能够在表达式中完成。C程序员可能会对此有所抱怨，但是这避免了在C中经常遇到的一类问题：误把```==```打成了```=```。

### 5.8 比较序列和其他的类型
序列的实例可以与同一序列类型的实例相比较，这一比较使用字典序：首先头两个元素比较，如果它们两个是不同的，则输出比较结果。如果它们是相同的，则比较后两个元素，如此继续下去。直到有一个序列到了末尾为止。如果要被比较的两个元素本身就是同种的序列，字典序的比较将会被递归地执行。如果两个序列中所有元素都相等，这两个序列就被认为是相等的，如果一个序列是另一个序列的子序列，较短的序列就被认为是更短（更小）的一个。对于字符串的字典序来说，它使用Unicode编码来比较每一个字符。下面是一个关于相同类型的序列的比较的例子：
```
(1, 2, 3)              < (1, 2, 4)
[1, 2, 3]              < [1, 2, 4]
'ABC' < 'C' < 'Pascal' < 'Python'
(1, 2, 3, 4)           < (1, 2, 4)
(1, 2)                 < (1, 2, -1)
(1, 2, 3)             == (1.0, 2.0, 3.0)
(1, 2, ('aa', 'ab'))   < (1, 2, ('abc', 'a'), 4)
```
注意：只要提供了合适的比较的方法，使用```<```或者```>```来比较两个实例是可以的。例如，混合的数值类型可以根据数值的大小来比较，所以0和0.0是相等的等等。否则的话，编译器将报出TypeError的异常，而不是提供一个任意的顺序来比较。

### 脚注
[1] 其他的语言可能会返回一个可变的实例，允许方法的链接，例如```d->insert("a")->remove("b")->sort();```
[2] 使用```d.keys()```将会返回一个字典视图实例。它支持一些诸如变量是否存在的检查和遍历操作，但是它的内容并不是独立于原有的字典的，它仅仅是一个视图。
