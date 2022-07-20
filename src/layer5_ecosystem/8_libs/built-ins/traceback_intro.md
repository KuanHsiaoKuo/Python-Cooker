# 异常追踪库

<!--ts-->
* [异常追踪库](#异常追踪库)
   * [基础例子](#基础例子)
   * [参考资源](#参考资源)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: runner, at: Wed Jul 20 03:35:21 UTC 2022 -->

<!--te-->
## 基础例子
~~~admonish info title='结合日志使用'
```python
import traceback
try:
    do_something()
except Exception:
    #traceback.print_exc(file=sys.stdout)
    loggers.info(traceback.format_exc())
```
~~~
## 参考资源

- [traceback — Print or retrieve a stack traceback — Python 3.10.5 documentation](https://docs.python.org/3/library/traceback.html)