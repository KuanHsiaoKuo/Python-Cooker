# 代码质量：异常、测试与日志

<!--ts-->
* [代码质量：异常、测试与日志](#代码质量异常测试与日志)
   * [调试：bdb与pdb](#调试bdb与pdb)
      * [Python标准库中的Debugger框架bdb模块](#python标准库中的debugger框架bdb模块)
         * [bdb模块的组成：](#bdb模块的组成)
      * [Python Debugger调试器](#python-debugger调试器)
         * [pdb模块中的Pdb类](#pdb模块中的pdb类)
         * [启动调试的方式](#启动调试的方式)
         * [调试命令](#调试命令)
   * [参考资源：](#参考资源)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: kuanhsiaokuo, at: Thu Jul  7 14:14:11 CST 2022 -->

<!--te-->

## 调试：bdb与pdb

### Python标准库中的Debugger框架bdb模块

> 提供基本的调试功能，如设置断点、管理Debugger的执行等。

#### bdb模块的组成：

- 异常bdb.BdbQuit(Exception)
- 类class bdb.Breakpoint
- 类class bdb.Bdb(skip=None)
- 测试类class Tdb(Bdb)

```admonish tip title='pdb是bdb的子类
Bdb是Python Debugger的基类，实际使用的Python Debugger是其子类Pdb。
```

- def set_trace(): 该方法用以开启调试。
- def checkfuncname(b, frame)
- def effective(file, line, frame): 该方法用以确定相对于该file的该line，哪个Breakpoint有效。

### Python Debugger调试器

> Python的一款交互式调试器，可以设置断点、单步调试、检查堆栈。

#### pdb模块中的Pdb类

```python
class Pdb(bdb.Bdb, cmd.Cmd)
```

#### 启动调试的方式

- Python解释器的命令行中

```python
# python3
import pdb

pdb.run('mymodule.mytest()')
```

- 调试一个脚本文件

```shell
python3 -m pdb myscript.py
```

- 在Python源代码中

```python
import pdb;

pdb.set_trace()
# ...
pdb.pm()
# 进入调试模式
```

#### 调试命令

- h,帮助
- w,打印堆栈
- d,在堆栈中移动到下一级frame
- u,在堆栈中移动到上一级frame

- b lineno|func,在指定位置处设置断点
- tbreak lineno|func,在指定位置处设置临时断点，执行时断点只生效一次
- disable bp_number,禁用指定断点
- enable bp_number,启动指定断点
- ignore bp_number count,忽略指定断点count次
- cl,清除所有断点
- cl lineno|func|bp_number,清除指定位置处的断点

- s,执行当前行，不进入被调用的函数
- n,执行到下一行，如果调用了其他函数则执行被调用函数
- unt,执行...直到
- r,执行到return
- c,继续执行
- j lineno,跳转到指定行

- l,
- ll,
- a,
- p ,
- pp ,
- whatis ,
- source ,
- display ,
- undisplay ,
- run,重启代码的执行
- restart,同run
- q,退出调试模式
- alias myname my_command,设置别名
- 通常在.pdbrc文件中
- unalias myname,取消别名
- interact

## 参考资源：

- [Python的调试框架bdb及调试器Pdb_易生一世的博客-CSDN博客](https://blog.csdn.net/taiyangdao/article/details/78287348)
- [bdb — Debugger framework — Python 3.10.5 documentation](https://docs.python.org/3/library/bdb.html)
- [pdb — The Python Debugger — Python 3.10.5 documentation](https://docs.python.org/3/library/pdb.html)
- [cpython/pdb.py at 3.5 · python/cpython](https://github.com/python/cpython/blob/3.5/Lib/pdb.py)
- [cpython/bdb.py at 3.5 · python/cpython](https://github.com/python/cpython/blob/3.5/Lib/bdb.py)