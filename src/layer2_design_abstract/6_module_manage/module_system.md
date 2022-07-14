# Python模块系统

<!--ts-->
* [Python模块系统](#python模块系统)
   * [package-&gt;module-&gt;method](#package-module-method)
      * [package](#package)
      * [module](#module)
      * [method](#method)
   * [模块的用意：分割与导入](#模块的用意分割与导入)
* [参考资源](#参考资源)
   * [<a href="https://docs.python.org/zh-cn/3/tutorial/modules.html" rel="nofollow">6. 模块 — Python 3.10.5 文档</a>](#6-模块--python-3105-文档)
   * [<a href="https://docs.python.org/zh-cn/3/reference/import.html" rel="nofollow">5. 导入系统 — Python 3.10.5 文档</a>](#5-导入系统--python-3105-文档)
      * [调用导入机制的方式](#调用导入机制的方式)
      * [导入模块发生了什么](#导入模块发生了什么)
      * [python的模块](#python的模块)
      * [python的包](#python的包)
      * [对比联系包和模块](#对比联系包和模块)
      * [包的分类](#包的分类)
         * [常规包](#常规包)
         * [命名空间包](#命名空间包)
      * [搜索](#搜索)
      * [加载](#加载)
      * [相对导入](#相对导入)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: runner, at: Thu Jul 14 06:52:33 UTC 2022 -->

<!--te-->

## package->module->method

### package

- [术语对照表:package — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/glossary.html#term-package)

> 一种可包含子模块或递归地包含子包的 Python module。从技术上说，包是带有 __path__ 属性的 Python 模块。

- [术语对照表:regular package — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/glossary.html#term-regular-package)

> 传统型的 package，例如包含有一个 __init__.py 文件的目录。

- [术语对照表: namespace package — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/glossary.html#term-namespace-package)

> PEP 420 所引入的一种仅被用作子包的容器的 package，命名空间包可以没有实体表示物，其描述方式与 regular package 不同，因为它们没有 __init__.py 文件。

- [PEP 420 – Implicit Namespace Packages | peps.python.org](https://peps.python.org/pep-0420/)

### module

- [术语对照表: module — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/glossary.html#term-module)

> 此对象是 Python 代码的一种组织单位。各模块具有独立的命名空间，可包含任意 Python 对象。模块可通过 importing 操作被加载到 Python 中。

- [术语对照表: module spec — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/glossary.html#term-module-spec)

> 一个命名空间，其中包含用于加载模块的相关导入信息。是 importlib.machinery.ModuleSpec 的实例。

```admonisth info title='ModuleSpec'
- [importlib --- import 的实现 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/library/importlib.html#importlib.machinery.ModuleSpec)
关于模块导入系统相关状态的规范。通常这是作为模块的 __spec__ 属性暴露出来。 
在以下描述中，括号里的名字给出了模块对象中直接可用的属性。
比如 module.__spec__.origin == module.__file__。 
但是请注意，虽然 值 通常是相等的，但它们可以不同，因为两个对象之间没有进行同步。
因此 __path__ 有可能在运行时做过更新，而这不会自动反映在 __spec__.submodule_search_locations 中。
```

### method

```admonish info title='MRO'
- [术语对照表: method resolution order — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/glossary.html#term-method-resolution-order)
方法解析顺序就是在查找成员时搜索全部基类所用的先后顺序. 请查看 Python 2.3 方法解析顺序 了解自 2.3 版起 Python 解析器所用相关算法的详情。
- [The Python 2.3 Method Resolution Order | Python.org](https://www.python.org/download/releases/2.3/mro/)
```

## 模块的用意：分割与导入

# 参考资源

## [6. 模块 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/tutorial/modules.html)

1. 文件即模块：为实现切分需求，Python 把各种定义存入一个文件，在脚本或解释器的交互式实例中使用。这个文件就是 模块 ；模块中的定义可以 导入 到其他模块或 主 模块（在顶层和计算器模式下，执行脚本中可访问的变量集）。

> 在模块这一点，python、rust和go各有不同

- python的代码管理最小单位是模块
- rust代码管理最小单位也是模块，但是它还提供了一个关键字mod让一个文件里面包含多个模块。rust的crate其实是'标准'包，由一系列已发布、版本化、被分发的包构成。可以直接从版本控制仓库或crate.io下载模块。
- go的module和package的关系则与前两者反过来：模块由一系列已发布、版本化、被分发的包构成。可以直接从版本控制仓库或模块代理服务器下载模块。

2. 文件名即模块名：模块是包含 Python 定义和语句的文件。其文件名是模块名加后缀名 .py 。
3. 在模块内部，通过全局变量 __name__ 可以获取模块名（即字符串）。
4. 关于模块命名空间：每个模块有各自文件范围内的全局命名空间(global namespace)
5. 模块顶部引入的其他模块在文件内全局可用，在函数、类等内部引入的，只能在内部使用
6. 关于from module_name import *: 导入所有不以下划线（_）开头的名称

> 大多数情况下，不要用这个功能，这种方式向解释器导入了一批未知的名称，可能会覆盖已经定义的名称。

7. 关于导入时发生的事情：

- 当一个名为 spam 的模块被导入时，解释器首先搜索具有该名称的内置模块。
- 这些模块的名字被列在 sys.buildin_module_names 中。
- 如果没有找到，它就在变量 sys.path 给出的目录列表中搜索一个名为 spam.py 的文件。

8. sys.path 从这些位置初始化:

- 输入脚本的目录（或未指定文件时的当前目录）。
- PYTHONPATH （目录列表，与 shell 变量 PATH 的语法一样）。
- 依赖于安装的默认值（按照惯例包括一个 site-packages 目录，由 site 模块处理）。

9. 关于'编译'文件处理：
   [6. 模块 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/tutorial/modules.html#compiled-python-files)
10. 找不到模块时的处理步骤：

- 使用dir()函数确认搜索路径
- 如果没找到，使用sys.path将对应路径加入

> dir()函数列出会所有类型的名称：变量、模块、函数等。但不会列出内置函数和变量的名称。这些内容的定义在标准模块 builtins 里

11. 包是模块的集合，存在形式是目录。导入包时，Python 搜索 sys.path 里的目录，查找包的子目录。
12. 包的标志: 必须有__path__属性，包含__init__.py
13. 包的分类：

- 传统包(regular package): 包含一个__init__.py

> 导入包的时候，或默认执行__init__.py里面的代码

- 命名空间包(namespace package): 一种仅被用作子包的容器的 package，命名空间包可以没有实体表示物, 没有__init__.py

14. 从包导入*

- 在文件系统查找并导入包的所有子模块。这项操作花费的时间较长，并且导入子模块可能会产生不必要的副作用，这种副作用只有在显式导入子模块时才会发生。
- 可以在__init__.py里面使用__all__定义默认可以被导入的模块

15. 包的__path__属性：在导入包的时候，执行__init__.py之前，会默认将这个属性进行初始化。主要是将当前导入包的目录放入搜索路径里面

- [__path__属性 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/reference/import.html#path__)
- [module.__path__ — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/reference/import.html#package-path-rules)

## [5. 导入系统 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/reference/import.html)

### 调用导入机制的方式

1. import语句/__import__()
2. importlib.import_module

```admonish info title='importlib说明'
importlib 模块提供了一个丰富的 API 用来与导入系统进行交互。 
例如 importlib.import_module() 提供了相比内置的 __import__() 更推荐、更简单的 API 用来发起调用导入机制。 
> 更多细节请参看 importlib 库文档:

[importlib --- import 的实现 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/library/importlib.html#module-importlib)
```

### 导入模块发生了什么

1. 当一个模块首次被导入时，Python 会搜索该模块
2. 如果找到就创建一个 module 对象 1 并初始化它。
3. 如果指定名称的模块未找到，则会引发 ModuleNotFoundError。
4. 当发起调用导入机制时，Python 会实现多种策略来搜索指定名称的模块。

> 这些策略可以通过使用使用下文所描述的多种钩子来加以修改和扩展。

```admonish tip title='3.3版更改'
在 3.3 版更改: 导入系统已被更新以完全实现 PEP 302 中的第二阶段要求。 
不会再有任何隐式的导入机制 —— 整个导入系统都通过 sys.meta_path 暴露出来。
此外，对原生命名空间包的支持也已被实现 (参见 PEP 420)。
```

### python的模块

Python 只有一种模块对象类型，所有模块都属于该类型，无论模块是用 Python、C 还是别的语言实现。

### python的包

为了帮助组织模块并提供名称层次结构，Python 还引入了 包 的概念。

你可以把包看成是文件系统中的目录，并把模块看成是目录中的文件

```admonish warn title="但请不要对这个类比做过于字面的理解"
因为包和模块不是必须来自于文件系统。 为了方便理解本文档，我们将继续使用这种目录和文件的类比。 
与文件系统一样，包通过层次结构进行组织，在包之内除了一般的模块，还可以有子包。
```

### 对比联系包和模块

```admonish tip title='包只是特殊的模块，具有__path__属性'
要注意的一个重点概念是所有包都是模块，但并非所有模块都是包。 或者换句话说，包只是一种特殊的模块。 特别地，任何具有 __path__ 属性的模块都会被当作是包。

所有模块都有自己的名字。 子包名与其父包名会以点号分隔，与 Python 的标准属性访问语法一致。
```

### 包的分类

#### 常规包

1. 包含__init__.py文件，会在导入时隐式执行
2. 如果没有__init__.py, 一个包含文件的目录也是会被认为是一个包, **命名空间包**

~~~admonish info title='多层__init__.py的执行顺序'
```shell
parent/
    __init__.py
    one/
        __init__.py
    two/
        __init__.py
    three/
        __init__.py
```

```python
import parent.one
```
1. 导入 parent.one 将隐式地执行 parent/__init__.py 和 parent/one/__init__.py。 
2. 后续导入 parent.two 或 parent.three 则将分别执行 parent/two/__init__.py 和 parent/three/__init__.py。

~~~

#### 命名空间包

- [10.5 利用命名空间导入目录分散的代码 — python3-cookbook 3.0.0 文档](https://python3-cookbook.readthedocs.io/zh_CN/latest/c10/p05_separate_directories_import_by_namespace.html)

1. 命名空间包不包含__init__.py, 这样就不会限定是单个包内的模块名称
2. 不包含__init__.py的时候，不同目录下的同名包会被认为是同一个包。

> 这一点可以和golang的包机制对比联系起来

~~~admonish info title='命名包的命名空间, 两个不同的包目录被合并到一起，你可以导入spam.blah和spam.grok，并且它们能够工作。'
```shell
foo-package/
    spam/
        blah.py

bar-package/
    spam/
        grok.py
```
```shell
>>> import sys
>>> sys.path.extend(['foo-package', 'bar-package'])
>>> import spam.blah
>>> import spam.grok
>>>
```
~~~

### 搜索

### 加载

### 相对导入

~~~admonish info title='举例相对导入'
```shell
package/
    __init__.py
    subpackage1/
        __init__.py
        moduleX.py
        moduleY.py
    subpackage2/
        __init__.py
        moduleZ.py
    moduleA.py
```
- 使用"."相对导入
```python
from .moduleY import spam
from .moduleY import spam as ham
from . import moduleY
from ..subpackage1 import moduleY
from ..subpackage2.moduleZ import eggs
from ..moduleA import foo
```

- 还可以主动添加搜索路径后导入
```python
# moduleX.py
import sys
sys.append('..')
from moduleA import *
```
~~~