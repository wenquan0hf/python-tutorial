# 13. 交互式输入行编辑历史回溯

有些版本的 Python 解释器支持输入行编辑和历史回溯，类似 Korn shell 和 GNU bash shell 的功能。这是通过 [GNU Readline](http://tiswww.case.edu/php/chet/readline/rltop.html) 库实现的。它支持 Emacs 风格和 vi 风格的编辑。这个库有它自己的文档，在此不重复了。

## 13.1.tab完成和历史编辑

自动完成变量和模块名在解释器启动时[自动启用](https://docs.python.org/3/library/site.html#rlcompleter-config)，这样Tab键调用完成功能；它着眼于　Python 语句的命名，当前的局部变量，有效的模块名。对于类似 string.a 这样的文件名，它会解析为 '.' 相关的表达式，从返回的结果对象中获取属性，以提供完成建议。需要注意的是，如果一个对象` __getattr__（）`方法是此表达式的一部分，这可能会执行应用程序定义代码。默认的配置还可以保存你的历史到 python_history 用户目录中，在下次交互式解释器会话期间，历史能够再次被使用。

## 13.2. 其它交互式解释器

跟早先版本的解释器比，现在已经有了很大的进步。不过，还是有些期待没有完成：它应该在后继行中优美的提供缩进（解释器知道下一行是否需要缩进）建议。完成机制可以使用解释器的符号表。命名检查（或进一步建议）匹配括号、引号等等。

另有一个强化交互式解释器已经存在一段时间了，它就是 [IPython](http://ipython.scipy.org/)，它支持 tab 完成，对象浏览和高级历史管理。它也可以完全定制或嵌入到其它应用程序中。另一个类似的强化交互环境是　[bpython](http://www.bpython-interpreter.org/) 。

