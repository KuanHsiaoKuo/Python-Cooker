# 异常追踪库

<!--ts-->
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