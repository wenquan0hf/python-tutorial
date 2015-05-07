
# 7. 输入和输出

我们有两种大相径庭地输出值方法：表达式语句 和 print() 函数。（第三种访求是使用文件对象的 write() 方法，标准文件输出可以参考sys.stdout 。详细内容参见库参考手册。）

通常，你想要对输出做更多的格式控制，而不是简单的打印使用空格分隔的值。 有两种方法可以格式化你的输出： 第一种方法是由你自己处理整个字符串，通过使用字符串切割和连接操作可以创建任何你想要的输出形式。string 类型包含一些将字符串填充到指定列宽度的有用操作，随后就会讨论这些。 第二种方法是使用 str.format() 方法。

标准模块 string 包括了一些操作，将字符串填充入给定列时，这些操作很有用。随后我们会讨论这部分内容。第二种方法是使用 Template 方法。

当然，还有一个问题，如何将值转化为字符串？很幸运，Python 有办法将任意值转为字符串：将它传入 repr() 或 str() 函数。

函数 str() 用于将值转化为适于人阅读的形式，而 repr() 转化为供解释器读取的形式（如果没有等价的语法，则会发生 SyntaxError 异常） 某对象没有适于人阅读的解释形式的话， str() 会返回与 repr() 等同的值。很多类型，诸如数值或链表、字典这样的结构，针对各函数都有着统一的解读方式。字符串和浮点数，有着独特的解读方式。

下面有些例子:  

```

>>> s = 'Hello, world.'  
>>> str(s)  
'Hello, world.' 
>>> repr(s)  
"'Hello, world.'"  
>>> str(1/7)  
'0.14285714285714285'  
>>> x = 10 * 3.25  
>>> y = 200 * 200  
>>> s = 'The value of x is ' + repr(x) + ', and y is ' + repr(y) + '...'  
>>> print(s)  
The value of x is 32.5, and y is 40000...  
>>> # The repr() of a string adds string quotes and backslashes:  
... Hello = 'hello, world\n'  
>>> hellos = repr(hello)  
>>> print(hellos) 
'hello, world\n'  
>>> # The argument to repr() may be any Python object:  
... repr((x, y, ('spam', 'eggs')))  
"(32.5, 40000, ('spam', 'eggs'))"  

```

有两种方式可以写平方和立方表:

```

>>> for x in range(1, 11):  
...      print(repr(x).rjust(2), repr(x*x).rjust(3), end=' ')  
...     # Note use of 'end' on previous line  
...     print(repr(x*x*x).rjust(4))  
...   
1   1    1  
 2   4    8   
3   9   27   
4  16   64   
5  25  125   
6  36  216   
7  49  343   
8  64  512   
9  81  729  
10 100 1000  
>>> for x in range(1, 11):  
...      print('{0:2d} {1:3d} {2:4d}'.format(x, x*x, x*x*x))  
...   
1   1    1   
2   4    8   
3   9   27   
4  16   64   
5  25  125  
 6  36  216   
7  49  343   
8  64  512  
9  81  729  
10 100 1000  

```

(注意第一个例子， print 在每列之间加了一个空格，它总是在参数间加入空格。)

以上是一个` str.rjust()` 方法的演示，它把字符串输出到一列，并通过向左侧填充空格来使其右对齐。类似的方法还有 `str.ljust()` 和`str.center()` 。这些函数只是输出新的字符串，并不改变什么。如果输出的字符串太长，它们也不会截断它，而是原样输出，这会使你的输出格式变得混乱，不过总强过另一种选择（截断字符串），因为那样会产生错误的输出值。（如果你确实需要截断它，可以使用切割操作，例如： `x.ljust(n)[:n]` 。）  

还有另一个方法，` str.zfill()` 它用于向数值的字符串表达左侧填充 0。该函数可以正确理解正负号:  

```
>>> '12'.zfill(5)  
'00012'  
>>> '-3.14'.zfill(7)  
'-003.14'  
>>> '3.14159265359'.zfill(5)  
'3.14159265359'  

```

方法 `str.format()` 的基本用法如下: 

```
>>> print('We are the {} who say "{}!"'.format('knights', 'Ni'))  
We are the knights who say "Ni!"  

```

大括号和其中的字符会被替换成传入 `str.format()` 的参数。大括号中的数值指明使用传入 `str.format()` 方法的对象中的哪一个。  

```
>>> print('{0} and {1}'.format('spam', 'eggs')) 
spam and eggs   
>>> print('{1} and {0}'.format('spam', 'eggs'))  
eggs and spam  

```

如果在` format()` 调用时使用关键字参数，可以通过参数名来引用值

```
>>> print('This {food} is {adjective}.'.format(  
...      food='spam', adjective='absolutely horrible'))  
This spam is absolutely horrible.  

```

定位和关键字参数可以组合使用:  

```

>>> print('The story of {0}, {1}, and {other}.'.format('Bill', 'Manfred',                                                                   other='Georg'))  
The story of Bill, Manfred, and Georg.  

```

`'!a'` (应用 `ascii()`), `'!s'` （应用 `str()` ） 和 `'!r'` （应用 `repr()` ） 可以在格式化之前转换值:  

```
>>> import math  
>>> print('The value of PI is approximately {}.'.format(math.pi))  
The value of PI is approximately 3.14159265359.  
>>> print('The value of PI is approximately {!r}.'.format(math.pi))  
The value of PI is approximately 3.141592653589793.  

```

字段名后允许可选的 `':'` 和格式指令。这允许对值的格式化加以更深入的控制。下例将 Pi 转为三位精度。 

```

>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 7678}  
>>> for name, phone in table.items():  
...     print('{0:10} ==> {1:10d}'.format(name, phone))  
...Jack       ==>       4098  
Dcab       ==>       7678  
Sjoerd     ==>       4127  

```

如果你有个实在是很长的格式化字符串，不想分割它。如果你可以用命名来引用被格式化的变量而不是位置就好了。有个简单的方法，可以传入一个字典，用中括号访问它的键:  

```

>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}  
>>> print('Jack: {0[Jack]:d}; Sjoerd: {0[Sjoerd]:d}; '  
...     'Dcab: {0[Dcab]:d}'.format(table))  
Jack: 4098; Sjoerd: 4127; Dcab: 8637678  

```

也可以用` ‘**’` 标志将这个字典以关键字参数的方式传入。

```
>>> table = {'Sjoerd': 4127, 'Jack': 4098, 'Dcab': 8637678}  
>>> print('Jack: {Jack:d}; Sjoerd: {Sjoerd:d}; Dcab:   {Dcab:d}'.format(**table))  
Jack: 4098; Sjoerd: 4127; Dcab: 8637678  

```

这种方式与新的内置函数 `vars()` 组合使用非常有效。该函数返回包含所有局部变量的字典。

要进一步了解字符串格式化方法 `str.format()` ，参见 [formatstrings]() 。

### 7.1.1. 旧式的字符串格式化

操作符 % 也可以用于字符串格式化。它以类似 `sprintf()-style` 的方式解析左参数，将右参数应用于此，得到格式化操作生成的字符串，例如:

```

>>> import math  
>>> print('The value of PI is approximately %5.3f.' % math.pi)  
The value of PI is approximately 3.142.  

```

进一步的信息可以参见　[string-formatting]() 一节。

## 7.2. 文件读写

函数 open() 返回文件对象，通常的用法需要两个参数：` open(filename, mode) `.

```

>>> f = open('workfile', 'w')  

```

第一个参数是一个标识文件名的字符串。第二个参数是由有限的字母组成的字符串，描述了文件将会被如何使用。可选的 模式 有：` 'r'` ，此选项使文件只读； `'w' `，此选项使文件只写（对于同名文件，该操作使原有文件被覆盖）；` 'a' `，此选项以追加方式打开文件；` 'r+'` ，此选项以读写方式打开文件； 模式 参数是可选的。如果没有指定，默认为 `'r'` 模式。

通常,文件是在文本模式下打开,这意味着,你读和写字符串的时候，文件编码在一个特定的编码(默认是utf - 8)。“b”添加到模式打开文件以二进制模式打开：现在表单中的数据读取和写入的字节对象。这种模式应该用于所有文件,不包含文本。 

在文本模式下,阅读时默认转换特定于平台的线末梢(`\ n` 在 `Unix`上,`\ r` ` \ n` 在 Windows上)`\ n`。当你写在文本模式下的时候，,默认的是出现 `\ n `转换回特定于平台的线的结局。这个幕后修改文件数据文本文件是好的，,但是腐败的二进制数据就像 JPEG 或 EXE 文件。当你在使用二进制模式阅读和写作时要非常仔细才可以。

### 7.2.1. 文件对象方法

本节中的示例都默认文件对象 f 已经创建。

要读取文件内容，需要调用 `f.read(size)` ，该方法读取若干数量的数据并以字符串形式返回其内容， size 是可选的数值，指定字符串长度。如果没有指定 size 或者指定为负数，就会读取并返回整个文件。当文件大小为当前机器内存两倍时，就会产生问题。反之，会尽可能按比较大的size 读取和返回数据。如果到了文件末尾，`f.read()` 会返回一个空字符串（”“）。

```

>>> f.read()'  
This is the entire file.\n'  
>>> f.read()  
''  

```

`f.readline()` 从文件中读取单独一行，字符串结尾会自动加上一个换行符（ `\n `），只有当文件最后一行没有以换行符结尾时，这一操作才会被忽略。这样返回值就不会有混淆，如果如果 `f.readline()` 返回一个空字符串，那就表示到达了文件末尾，如果是一个空行，就会描述为`'\n' `，一个只包含换行符的字符串。

```

>>> f.readline()  
'This is the first line of the file.\n'  
>>> f.readline()  

'Second line of the file\n'  
>>> f.readline()  
''  

```

对于从文件读取行,可以遍历文件对象。这是内存高效、快速和导致简单的代码:

```

>>> for line in f  
:...      print(line, end='')  
...This is the first line of the file.Second line of the file  

```

如果你想阅读列表中的所有行的文件，你还可以使用列表(`f`)或`f.readlines()`。

`f.write(string)` 将字符串的内容写入文件,返回字符数。

```

>>> f.write('This is a test\n')  
15  

```

想要写入其他非字符串内容，首先要将它转换为字符串:  

```

>>> value = ('the answer', 42)  
>>> s = str(value)  
>>> f.write(s)18   
 
```

`f.tell()` 返回一个整数，代表文件对象在文件中的指针位置，在文本模式下，该数值计量了自文件开头到指针处的比特数。

需要改变文件对象指针话话，使用 `f.seek(offset,from_what)` 。指针在该操作中从指定的引用位置移动 offset 比特，引用位置由 `from_what` 参数指定。 `from_what` 值为 0 表示自文件起始处开始，1 表示自当前文件指针位置开始，2 表示自文件末尾开始。 `from_what` 可以忽略，其默认值为零，此时从文件头开始。  

```

>>> f = open('workfile', 'rb+')  
>>> f.write(b'0123456789abcdef')  
16  
>>> f.seek(5)   # Go to the 6th byte in the file
5  
>>> f.read(1)  
b'5'  
>>> f.seek(-3, 2) # Go to the 3rd byte before the end
13  
>>> f.read(1)  
b'd'  

```

在文本文件中（那些没有使用 b 模式选项打开的文件），只允许从文件头开始计算相对位置（使用 `seek(0, 2)` 从文件尾计算时就会引发异常）。

当你使用完一个文件时，调用 `f.close()` 方法就可以关闭它并释放其占用的所有系统资源。 在调用 `f.close()` 方法后，试图再次使用文件对象将会自动失败。

```

>>> f.close()  
>>> f.read()  
Traceback (most recent call last):  
  File "<stdin>", line 1, in ?  
ValueError: I/O operation on closed file  

```

用关键字 with 处理文件对象是个好习惯。它的先进之处在于文件用完后会自动关闭，就算发生异常也没关系。它是 `try-finally` 块的简写:

```
>>> with open('workfile', 'r') as f:  
...    read_data = f.read()  
>>> f.closed  
True  

```

文件对象还有一些不太常用的附加方法，比如 `isatty()` 和 `truncate()` 在库参考手册中有文件对象的完整指南。

### 7.2.2.结构化数据保存与 json

我们可以很容易的读写文件中的字符串。数值就要多费点儿周折，因为 `read()` 方法只会返回字符串，应该将其传入 `int()` 这样的方法中，就可以将 `'123'` 这样的字符转为对应的数值 123。不过，当你需要保存更为复杂的数据类型，例如列表、字典，类的实例，事情就会变得更复杂了。

好在用户不必要非得自己编写和调试保存复杂数据类型的代码。 Python 提供了一个名为 `pickle` 的标准模块。这是一个令人赞叹的模块，几乎可以把任何 Python 对象 （甚至是一些 Python 代码段！）表达为为字符串，这一过程称之为封装 （ pickling ）。从字符串表达出重新构造对象称之为拆封（ unpickling ）。封装状态中的对象可以存储在文件或对象中，也可以通过网络在远程的机器之间传输。

笔记：JSON 格式是被现代的应用程序使用允许数据交换。许多程序员已经熟悉它，对于相互操作性，这是一个很好的选择。

如果你有一个对象 x ，一个以写模式打开的文件对象 f ，封装对象的最简单的方法只需要一行代码:

```

>>> json.dumps([1, 'simple', 'list'])  
'[1, "simple", "list"]'  

```

另一个变体转储函数,称为 dump(),可以将对象序列化到一个文本文件。所以如果f是一个写模式打开的文本文件对象，我们可以这样做:

```

json.dump(x, f)

```


第二次解码,如果f是一个读模式打开的文本文件对象:

```
x = json.load(f)

```

这个简单的序列化技术可以处理列表和字典,但在 JSON 序列化任意类实例需要一点额外的努力。json的参考模块包含一个解释。

参见 pickle,pickle 模块
与 JSON 相反,pickle 是一个协议,允许序列化任意复杂的 Python 对象。因此,它是特定于 Python 和不能用于与其他语言编写的应用程序通信。也是不安全的默认:如果数据被老练的攻击者攻击，来自一个不可信的源的 pickle 数据可以执行任意代码。

>
撰写时请删除此段

原文地址：  
https://docs.python.org/3/tutorial/inputoutput.html

