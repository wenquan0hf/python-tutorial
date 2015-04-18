# 2. 使用Python解释器


## 2.1 调用解释器
  通常 Python 的解释器被安装在可用的目标机器/usr/local/bin/python3.4 目录下；把/usr/local/bin 目录放进你的 Unix Shell 的搜索路径里，通过输入命令来运行它。 
 
```
Python3.4
```

   对 shell.[1] 来说，由于安装路径是可以选择的，所以也有可能安装在其它位置，你可以与当地的 Python 专家或者系统管理员联系。（例如，/usr/local Python 就是一个受欢迎的替代位置。）  

   在Windows机器上，Python通常安装在C:\Python34，但当我们运行安装程序的时候可以改变它。为了把这个目录加入到环境变量中，你需要在DOS窗口中键入如下命令行：  

```
Set path=%path%;C:\python34
```

   输入一个文件结束符（Unix 上是 Ctrl+D，Windows 上是 Ctrl+Z）解释器会以 0 值退出。如果这样没有起作用，你可以输入以下命令来退出：quit()。  

解释器的命令行编辑功能通常并不复杂。 装在 Unix 上的解释器可能会有 GNU readline 库的支持，  这样就能够得到更加精巧的交互编辑和历史记录特性。检查命令行编辑特性是否支持的最快方式是在 Python 解释器的第一个提示符后输入 Ctrol-P。如果有嘟嘟声，说明你可以使用命令后编辑功能，参阅附录[Interactive Input Editing ang History Substitution](https://docs.python.org/3/tutorial/interactive.html#tut-interacting)获得更多关于此的信息。如果任何事情也没有发生或者只是出现一个 ^p 字符，说明命令行编辑功能不可用，你只有用退格键来删除输入的命令。  

  解释器工作起来和 Unix 的 Shell 类似：使用终端设备作为标准输入来调用她时，解释器交互的解读和执行命令，通过文件名参数或以文件作为标准输入设备时，它从文件中解读并执行脚本。 

   启动解释器的第二个方法是```python-c command[arg]....```,这种方法可以在命令行中直接执行语句，等同于shell的-c选项。因为Python语句通常会包括空格之类的特殊字符，所以最好把整个命令用单引号包括起来。  

  有些 Python 模块也可以当做脚本使用，它们可以用```python-m module[arg]...```的方式调用，这样就会像你在命令行中给出其完整名字一样运行模块源文件。  

  使用脚本文件时，经常会运行脚本然后进入交互模式。这也可以通过在脚本之前加上-i参数来实现。

### 2.1.1 参数传递 

  调用解释器时，脚本名和附加参数将变成一个字符串列表，并赋值到 sys 模块的 argv 变量中。你可以通过执行 import sys 来访问此列表。列表的长度至少为1；当没有脚本并没有给出参数时，sys.[ 0 ] 是一个空字符串。当脚本名为“-”（意思是标准输入），则 sys.argv [ 0 ] 设置为“-” 。当使用 -c 命令时，sys.argv [ 0 ] 被设置为“-C”。当 -m 模块时，sys.argv [ 0 ] 被设置为完整的模块地址名称。-C 命令或-M 模块之后发现选项不会被 Python 解释器的选项处理消耗，而是由留在 sys.argv 中的命令或模块来处理。
### 2.1.2 交互模式

  从 tty 读取命令时，我们称解释器工作于交互模式。这种模式下它根据主提示符来执行，主提示符通常标识为三个大于括号（>>>）；继续的部分被称为从属提示符，由三个点标识（...）。在第一行前，解释器打印欢迎信息、版本号和授权提示

```
$ python3.4  
Python 3.4 (default Mar 16 2014,09:25:04)  
[GCC 4.8.2] on linux  
Type”help”,”copyrigt”,”credits”or”license”for more information.  
>>>  
```

  输入多行结构是需要从属提示符了，例如，下面举的这个if语句
  
```
>>> the_word_is_flat=True  
>>> if the_word_is_flat:  
...   Print(“Be careful not to fall off !”)  
...  
Be careful no to fall off !  
```

更多的交互模式，请看[Iinteractive Mode](https://docs.python.org/3/tutorial/appendix.html#tut-interac). 

## 2.2 解释器及其环境

### 2.2.1 源代码编码

  默认情况下，Python 源文件时用UTF-8来编码的。在此编码下，世界上的大多数语言的字符可以同时使用字符串、标识符和注释中—尽管 Python 标准库仅使用ASCLL字符作为标识符，这只是任何可移植代码应该遵守的约定。如果要正确显示所有的字符，你的编辑器必须能够识别出文件时 UTF-8 编码，并且她使用的字体能支持文件中所有的字符。

  你也可以为源文件指定不同的字符编码。为了这样做，在 #! 行（首行）后插入至少一行特殊的注释行来定义源文件的编码。  

```
 #—*—coding: encoding —*—
```

通过此声明，源文件中所有的东西都会被当做用 encoding 指代的 UTF-8 编码对待。在 Python 库参考手册 codecs 一节中你可以找到一张可以用的编程列表。

例如，如果你的编辑器不支持UTF-8编码的文件，但支持像 windows-1252 的其他一些编码，你可以定义：
```
 #—*— coding: cp-1252 —*—
```

 这样就可以在源文件中使用 Windows-1252 字符集中的所有字符了。这个上的编码注释必须在文件中的第一或者第二行进行定义。

**脚注** [1]  在Unix系统中,为了不使与它同时安装的 Python 2x 出现执行冲突，Python 解释器 3.x 不安装，默认执行 python。