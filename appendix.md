# 附录
## 交互模式
### 错误处理

当错误发生时，解释器打印一个错误消息和堆栈跟踪。在交互模式下，它会返回到主提示符；如果输入来自一个文件，它会在打印堆栈信息后以非零状态退出（一个 try 语句中的 except 子句没有错误在这个背景下。处理例外）。一些错误是致命的，因为一个零状态退出；这适用于内部的矛盾和一些内存耗尽的情况下。所有的错误信息都写入标准错误流；正常情况下执行命令的输出写入标准输出。

输入中断符（通常是 Control-C or DEL）到原发性或继发性提示取消输入并返回到主提示符。**脚注**[1] 当命令正在执行时输入一个中断，提高了 keyboardinterrupt 异常，这可能是由一个 try 语句处理。
 
### 执行 Python 脚本

在 BSD Unix 系统中 Python 脚本通过将行可以直接执行，像 shell 脚本，

```
 #!/usr/bin/env python3.4
```

（假设解释是在用户的路径上）在脚本的开始，给文件的执行模式。#！必须是文件的前两个字符。在一些平台上，该行必须以 UNIX 风格的行结束（`'\n'`），（`“\r\n”`）行结束。注意哈希，或磅字符，`’#’`在 python 中是被用于开始一个评论。

该脚本可以给出一个可执行的模式，或许可，使用 chmod 命令

```
$ chmod +x myscript.py
```

在 Windows 系统中，没有可执行模式。Python 安装程序自动将 .py 与 python.exe 文件联系起来，双击一个 Python 文件将作为一个脚本运行它。扩展也可以是 .pyw，在这种情况下，通常出现在控制台的窗口被抑制。

### 交互启动文件

当你使用 Python 解释器，在每次解释器启动的时候，它常常是有一定的标准执行的命令。你可以通过设置一个名为 pythonstartup 到包含有启动命令的文件名的环境变量中。这是类似于 UNIX 外壳的 .profile 中的功能。

这个文件在交互会话期是只读的，而不是在 Python 读取脚本命令时，更不是当 /dev/tty 为外部命令源时（否则就像一个交互式会话）。这是运行在同一个命名空间的交互式命令被执行，所以由它定义或进口可不使用交互式会话。你也可以在这个文件夹中改变 sys.ps1 和 sys.ps2 指令。

如果你想从当前目录读取附加的启动文件，你可以在全球启动文件中使用代码如 `os.path.isfile(“.pythonrc.py”):exec (open(.pythonrc.py).read())`。如果你想在一个脚本中使用的启动文件，你必须明确地在脚本中这样做：

```
Import os  
filename = os.environ.get('PYTHONSTARTUP')  
if filename and os.path.isfile(filename):  
  with open(filename) as fobj:  
     startup_file = fobj.read()  
  exec(startup_file)  
```

### 定制模块

Python 提供了两个自定义挂钩： sitecustomize 和 usercustomize 。为了看到它是如何工作的，第一你需要找到你你的用户网站的软件包目录的位置。启动 Python 并且运行这段代码。

```
>>> import site  
>>>> site.getusersitepackages()  
'/home/user/.local/lib/python3.4/site-packages'
```
 
现在你可以创建一个 usercustomize.py 的文件名在目录中，放任何你想要的东西在里面。它会影响每一个 Python 的调用，除非它是以与 -s 选项禁用自动进口开始。

sitecustomize 以同样的方式工作，但通常是通过全球网站包目录的计算机的管理员创建，并在之前usercustomize 导入。

**脚注**

[1] GNU Readline 包可能会阻止这个问题。


