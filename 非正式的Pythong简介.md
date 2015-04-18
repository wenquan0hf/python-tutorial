# 3.非正式的 Python 简介

下面的例子中，输入和输出分别由大于号和句号提示符（`>>>` 和 `...` ）：如果想重现这些例子，就要在解释器的提示符后，输入（提示符后面的）那些不包含提示符的代码行。需要注意的是在练习中遇到的从属提示符表示你需要在最后多输入一个空行，解释器才能知道这是一个多行命令的结束。  

本手册中的很多示例——包括那些带有交互提示符的——都含有注释。Python 中的注释以` #` 字符起始，直至实际的行尾。注释可以从行首开始，也可以在空白或代码之后，但是不出现在字符串中。文本字符串中的` # `字符仅仅表示 `# `。由于代码中的注释不会被 Python 解释，录入示例的时候可以忽略它们。

如下面这个例子：

```
 # this is the first comment  
spam = 1 # and this is the second comment  
           # ... and now a third!  
Text = "# This is not a comment because it's inside quotes." 

```

## 3.1. 将 Python 当做计算器使用

我们来尝试一些简单的 Python 命令。启动解释器然后等待主提示符` >>> `出现。（这不会花很长时间。）

### 3.1.1. 数字
解释器的表示就像一个简单的计算器：可以向其录入一些表达式，它会给出返回值。表达式语法很直白：运算符 `+`，` -` ，` *` 和 / 与其它语言一样（例如： Pascal 或 C）；括号（）可用于分组。例如:

```

>>> 2 + 2  
4  
>>> 50 - 5*6  
20
>>> (50 - 5*6) / 4  
5.0
>>> 8 / 5  # division always returns a floating point number  
1.6  

```

整数(如:2、4、20)为int类型,有小数点部分(如5.0、1.6)则为` float` 型。稍后我们将在后面的教程中看到更多的关于数字类型的教程。

除法(`/`)总是返回一个 `float` 类型。想要得到的一个整数(丢弃任何小数的结果)你可以使用`// `运算符;要计算余数你可以使用`%`:

```

>>> 17 / 3  # classic division returns a float  
5.666666666666667  
>>>  
>>> 17 // 3  # floor division discards the fractional part  
5  
>>> 17 % 3  # the % operator returns the remainder of the division  
2  
>>> 5 * 3 + 2 # result * divisor + remainder  
17  

```

在 Python 中，可以使用`* *`运算符来计算[1]:

```
>>> 5 ** 2  # 5 squared  
25  
>>> 2 ** 7   2 to the power of 7  
128  

```

等号(`=`)用于给一个变量赋值。后来,没有结果显示在下一个交互式提示符之前:

```
>>> width = 20  
>>> height = 5 * 9  
>>> width * height  
900  

```

Python 完全支持浮点数，不同类型的操作数混在一起时，操作符会把整型转化为浮点数。

```

>>> 3 * 3.75 / 1.5  
7.5  
>>> 7.0 / 2  
3.5  

```

在交互模式下，最后打印表达式赋给变量_。这意味着当你使用 Python 作为一个计算器，它是 继续计算比较容易，例如：

```
>>> tax = 12.5 / 100  
>>> price = 100.50   
>>> price * tax12.5625 
>>> price + _113.0625  
>>> round(_, 2)  
113.06  

```

此变量对于用户是只读的。不要尝试给它赋值 —— 你只会创建一个独立的同名局部变量，它屏蔽了系统内置变量的魔术效果.

除了` int` 和 `float`，Python 支持其他类型的数字，如小数和分数。Python 还内置了用于复数的支持，并使用J或j的后缀来表示虚部（如3+5J）。

### 3.1.2。字符串

除了数值，Python 也提供了可以通过几种不同方式传递的字符串。它们可以用单引号或双引号标识。他们可以用单引号（`“……” `）或双引号（`“……”`）可以得出相同的结果[ 2 ]。也可以使用  `\` 进行转义：

```

>>> 'spam eggs'  # single quotes  
'spam eggs'  
>>> 'doesn\'t'  # use \' to escape the single quote..."  
doesn't"  
>>> "doesn't"  # ...or use double quotes instead  
"doesn't"  
>>> '"Yes," he said.  
''"Yes," he said.'  
>>> "\"Yes,\" he said."  
'"Yes," he said.'  
>>> '"Isn\'t," she said.  
''"Isn\'t," she said.'  

```

在 Python 交互式解释器中，字符串的输出结果用引号括起来，特殊字符则通过 `\` 反斜杠进行转义。我们可能发现了它们可能和输入有点不一样(括起来的引号变了)，但是它们是相等的。如果字符串包含单引号并且不包含双引号，则输出结果用双引号括起来，否则用单引号括起来。 `print()` 函数输出字符串更具有可读性，它不会将输出结果用引号括起来。

```
>>> '"Isn\'t," she said.  
''"Isn\'t," she said.'  
>>> print('"Isn\'t," she said.')  
"Isn't," she said.  
>>> s  = 'First line.\nSecond line.'  # \n means newline  
>>> s  # without print(), \n is included in the output'  
First line.\nSecond line.'  
>>> print(s)  # with print(), \n produces a new line  
First line.  
Second line.  

```

如果你不想使用反斜杠 `\` 来转译特殊字符，那么你可以在原始字符串前加  `r` ：

```

>>> print('C:\some\name')  # here \n means newline!  
C:\someame  
>>> print(r'C:\some\name')  # note the r before the quote  
C:\some\name  

```

字符串常量可用跨越多行，其中一种使用方式是用三重引号： ` """  ... """ ` 或者 `  ''' ... ''' `。如果在第一个三引号后面不加反斜杠 `\`，则字符串之前会自动加一空行。可以用反斜杠 `\` 阻止这种行为。如下面的列子：

```

print("""\   
Usage: thingy [OPTIONS]  
     -h                 Display this usage message       
-H hostname               Hostname to connect to  
""")

```

下面是输出结果(注意，开始一行是没有空行的)：

```

 Usage: thingy [OPTIONS]       
 -h                        Display this usage message  
 -H hostname               Hostname to connect to

```

字符串可以通过+运算符进行链接，或者通过*进行重复链接：

```

>>> # 3 times 'un', followed by 'ium'  
>>> 3 * 'un' + 'ium'  
'unununium'  
```

两个或多个（即那些被引号引起来的部分）字符串常量则会自动进行连接运算：

```

>>> 'Py' 'thon'
'Python'
```

上面的操作仅适用于两个字符串常量，变量和常量是不可以连接的：

```

>>> prefix = 'Py'  
>>> prefix 'thon'  # can't concatenate a variable and a string literal  
  ...  
SyntaxError: invalid syntax  
>>> ('un' * 3) 'ium'  
  ...
SyntaxError: invalid syntax  

```

如果你想将变量和常量连接，则使用 `+` 运算符：

```

>>> prefix + 'thon'  
'Python'  

```

这种特征特别适用于长字符串的分行书写：

```

>>> text = ('Put several strings within parentheses '  
           'to have them joined together.')  
>>> text  
'Put several strings within parentheses to have them joined together.'  


```

字符串可以使用索引操作，第一个字符的索引为 0，Python中没有单独的字符类型，一个字符也字符串。


```

>>> word = 'Python'  
'P'>>> word[5]  # character in position 5  
'n'  

```

索引可以使负数，表示从字符串的右边开始进行索引：


```
>>> word[-1] # last character  
'n'  
>>> word[-2]  # second-last character   
'o'    
>>> word[-6]  
'P'  


```

注意，因为 -0 和 0 是一致的，因此负数索引是从 -1 开始。
除了索引之外，字符串还支持切片操作。索引用来表示单个字符，切片允许你包含子串：

```

>>> word[0:2] # characters from position 0 (included) to 2 (excluded)  
'Py'  
>>> word[2:5]  # characters from position 2 (included) to 5 (excluded)  
'tho'  

```

注意，切片遵守前闭后开原则。 ` s[:i] + s[i:] ` 总是等于 `s`:

```
>>> word[:2] + word[2:]   
'Python'  
>>> word[:4] + word[4:]  
'Python'  

```

索引切片可以使用默认值，切片时，忽略第一个索引的话，默认为 0，忽略第二个索引，默认为字符串的长度：

```

>>> word[:2]  # character from the beginning to position 2 (excluded)  
'Py'  
>>> word[4:]  # characters from position 4 (included) to the end  
'on'  
>>> word[-2:] # characters from the second-last (included) to the end  
'on'  

```

有一个办法可以记住切片的工作方式：切片时的索引是在两个字符之间 。左边第一个字符的索引为 0，而长度为 n 的字符串其最后一个字符的右界索引为 n 。例如

```
+---+---+---+---+---+---+  
| P | y | t | h | o | n  
+---+---+---+---+---+---+  
0   1   2   3   4   5   6  
-6  -5  -4  -3 -2  -1

```

文本中的第一行数字给出字符串中的索引点 0...6 。第二行给出相应的负索引。 切片是从 `i` 到 `j` 两个数值标示的边界之间的所有字符。

对于非负索引，如果上下都在边界内，切片长度就是索引值的差。例如，` word[1:3]` 是 2  

尝试使用一个索引过大就会出现错误。  


```

>>> word[42]  # the word only has 6 characters  
Traceback (most recent call last):    
 File "<stdin>", line 1, in <module>   
IndexError: string index out of range  

```

然而，当索引越界，切片操作可以很优雅的进行处理：  

```
>>> word[4:42]  
'on'  
>>> word[42:]  
''  

```

Python 字符串是不能改变的—它是不可变量，因此，给某个索引位置进行赋值是不行的：


```
>>> word[0] = 'J'  
  ...TypeError: 'str' object does not support item assignment  
>>> word[2:] = 'py'   
  ...TypeError: 'str' object does not support item assignment  

```

如果你需要一个不同的字符串，你应该创建一个新的：

```

>>> 'J' + word[1:]
'Jython'
>>> word[:2] + 'py'
'Pypy'

```

内置函数 `len()` 返回一个字符串的长度

```
>> s = 'supercalifragilisticexpialidocious'  
>>> len(s)  
34  

```

**参见**：

[文本类型STR序列](https://docs.python.org/3/library/stdtypes.html#textseq)

字符串属于序列类型 ，序列的常见操作都支持。

[字符串的方法](https://docs.python.org/3/library/stdtypes.html#string-methods)

字符串支持大量用于基本方法转换和搜索。

[字符串格式化](https://docs.python.org/3/library/string.html#string-formatting)

字符串格式化信息与 `str.format（）`这里描述。

[Printf格式化字符串](https://docs.python.org/3/library/stdtypes.html#old-string-formatting)

旧的格式化字符串和 Unicode 字符串时调用操作的左操作数%运算符进行更详细的描述。

### 3.1.3列表

Python 有几个复合 数据类型，用于表示其它的值。最通用的是 `list` (列表) ，它可以写作中括号之间的一列逗号分隔的值。列表的元素不必是同一类型。

```
>>> squares = [1, 4, 9, 16, 25]  
>>> squares  
[1, 4, 9, 16, 25]  

```

就像字符串索引（和所有其他内置的序列类型），列表可以被切片和连接:

```
>>> squares[0]  # indexing returns the item  
1  
>>> squares[-1]  
25  
>>> squares[-3:]  # slicing returns a new list  
[9, 16, 25]  

```

所有的切片操作都会返回新的列表，包含求得的元素。这意味着以下的切片操作返回列表 `a` 的一个浅拷贝的副本:

```
>>> squares[:]  
[1, 4, 9, 16, 25]  

```

列表还支持级联操作

```
>>> squares + [36, 49, 64, 81, 100]  
[1, 4, 9, 16, 25, 36, 49, 64, 81, 100]  

```

不像不可变的字符串，列表是可变类型，允许修改列表的内容:

```
>>> cubes = [1, 8, 27, 65,125]  # something's wrong here  
>>> 4 ** 3  # the cube of 4 is 64, not 65!  
64  
>>> cubes[3] = 64  # replace the wrong value  
>>> cubes  
[1, 8, 27, 64, 125]  

```

利用 `append()`方法，你还可以在列表末尾添加新的项目。（我们将在后面看到更多的方法）

```

>>> cubes.append(216)  # add the cube of 6  
>>> cubes.append(7 ** 3)  # and the cube of 7  
>>> cubes  
[1, 8, 27, 64, 125, 216, 343]  

```

也可以对切片赋值，此操作可以改变列表的尺寸，或清空它:

```
>>> letters = ['a', 'b', 'c', 'd', 'e', 'f', 'g']  
>>> letters  
['a', 'b', 'c', 'd', 'e', 'f', 'g']  
 >>> # replace some values  
>>> letters[2:5] = ['C', 'D', 'E']  
>>> letters  
['a', 'b', 'C', 'D', 'E', 'f', 'g']  
>>> # now remove them  
>>> letters[2:5]= []  
>>> letters['a', 'b', 'f', 'g']  
>>> # clear the list by replacing all the elements with an empty list  
>>> letters[:] = []  
>>> letters  
[]  

```

内置函数`len()` 也适用于列表：

```
>>> letters = ['a', 'b', 'c', 'd']  
>>> len(letters)  
4  
```

它可以嵌套列表（创建包含其他列表的列表），例如：

```
>>> a = ['a', 'b', 'c']  
>>> n = [1, 2, 3]  
>>> x = [a, n]  
>>> x  
[['a', 'b', 'c'], [1, 2, 3]]  
>>> x[0]['a', 'b', 'c']  
>>> x[0][1]'b'  

```

## 3.2 编程的第一步

当然，我们可以使用 Python 完成比二加二更复杂的任务。例如，我们可以写一个生成 菲波那契 子序列的程序，如下所示:

```
>>> # Fibonacci series:  
... # the sum of two elements defines the next  
... a, b = 0, 1  
>>> while b < 10:  
...     print(b)  
...     a, b = b, a+b  
...  
112358  

```

这个例子介绍了几个新功能。

* 第一行包括了一个 多重赋值 ：变量 `a `和 `b` 同时获得了新的值 0 和 1 最后一行又使用了一次。在这个演示中，变量赋值前，右边首先完成计算。右边的表达式从左到右计算。

* 条件（这里是 `b < 10` ）为 `true` 时，` while` 循环执行。在 Python 中，类似于 C ，任何非零整数都是 `true`；0 是 `false` 条件也可以是字符串或列表，实际上可以是任何序列；所有长度不为零的是 `true` ，空序列是 `false`。示例中的测试是一个简单的比较。标准比较操作符与 C 相同： `< `（小于），` > `（大于），` == `（等于），` <=` （小于等于），` >=` （大于等于）和 `!=` （不等于）。

 
* 循环体是缩进的：缩进是 Python 是 Python 组织語句的方法。 Python 不提供集成的行编辑功能，所以你要为每一个缩进行输入 `TAB `或空格。实践中建议你找个文本编辑来录入复杂的 Python 程序，大多数文本编辑器提供自动缩进。交互式录入复合语句时，必须在最后输入一个空行来标识结束（因为解释器没办法猜测你输入的哪一行是最后一行），需要 注意的是同一个语句块中的语句块必须缩进同样数量的空白。

* 关键字 `print` 语句输出给定表达式的值。它控制多个表达式和字符串输出为你想要字符串（就像我们在前面计算器的例子中那样）。字符串打印时不用引号包围，每两个子项之间插入空间，所以你可以把格式弄得很漂亮，像这样:

```
>>> i = 256*256  
>>> print('The value of i is', i)  
The value of i is 65536  

```

关键字参数端可用来避免输出后换行，或者用不同的字符串结束输出：

```
>>> a, b = 0, 1  
>>> while b < 1000:  
...  print(b, end=',')  
...    a, b  = b, a+b.  
..
1,1,2,3,5,8,13,21,34,55,89,144,233,377,610,987,  

```

**脚注**

[1] 由于`**`的优先级高于` -` ，`-3** 2` 将被解释为 `-（3**2）`，从而导致结果为 -9。为了避免这种情况，并得到 9，你可以使用`（-3）** 2`

[2] 与其他语言不同,特殊字符如`\ n`，具有相同含义的两个单引号(`'…'`)和双引号(`“…”`)。两者之间唯一的区别是在单引号不需要转码建 `”`(但你必须有转码键`\ '`)，反之亦然。