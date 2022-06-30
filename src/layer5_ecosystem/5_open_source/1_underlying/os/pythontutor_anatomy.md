# pythontutor项目解剖

<!--ts-->
* [pythontutor项目解剖](#pythontutor项目解剖)
   * [使用bottle写web服务](#使用bottle写web服务)
   * [pg_logger.py介绍](#pg_loggerpy介绍)
      * [继承自Bdb](#继承自bdb)
      * [初始化相关](#初始化相关)
      * [调用函数](#调用函数)
   * [参考资料](#参考资料)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: kuanhsiaokuo, at: Thu Jun 30 23:34:51 CST 2022 -->

<!--te-->

## 使用bottle写web服务

```python
from bottle import route, get, request, run, template, static_file

try:
    import StringIO  # NB: don't use cStringIO since it doesn't support unicode!!!
except:
    import io as StringIO  # py3
import json
import pg_logger


@route('/web_exec_<name:re:.+>.py')
@route('/LIVE_exec_<name:re:.+>.py')
@route('/viz_interaction.py')
@route('/syntax_err_survey.py')
@route('/runtime_err_survey.py')
@route('/eureka_survey.py')
@route('/error_log.py')
def dummy_ok(name=None):
    return 'OK'


@route('/<filepath:path>')
def index(filepath):
    return static_file(filepath, root='.')


# Note that this will run either Python 2 or 3, depending on which
# version of Python you used to start the server, REGARDLESS of which
# route was taken:
@route('/web_exec_py2.py')
@route('/web_exec_py3.py')
@route('/LIVE_exec_py2.py')
@route('/LIVE_exec_py3.py')
def get_py_exec():
    out_s = StringIO.StringIO()

    def json_finalizer(input_code, output_trace):
        ret = dict(code=input_code, trace=output_trace)
        json_output = json.dumps(ret, indent=None)
        out_s.write(json_output)

    options = json.loads(request.query.options_json)

    pg_logger.exec_script_str_local(request.query.user_script,
                                    request.query.raw_input_json,
                                    options['cumulative_mode'],
                                    options['heap_primitives'],
                                    json_finalizer)

    return out_s.getvalue()


if __name__ == "__main__":
    run(host='localhost', port=8003, reloader=True)
```

> 这里可以发现，作者的路由基本汇聚在get_py_exec, 进入后执行exec_script_str_local

## pg_logger.py介绍

~~~admonish info title='python tutor核心类'
```
┌───────────────────┐                                                        
│                   │                                                        
│ botter_server.py  │                                                        
│    get_py_exec    │──────────────┐                                         
│                   │              │                                         
└───────────────────┘              │                                         
                                   │                                         
                                   │              ┌─────────────────────────┐
                                   └──────────────▶                         │
                                                  │      pg_logger.py       │
                                                  │  exec_script_str_local  │
                                      ┌───────────┤                         │
                                      │           └─────────────────────────┘
                                      │                                      
                                      │                                      
                                      │                                      
┌─────────────────────────┐           │                                      
│                         ◀───────────┘                                      
│      pg_logger.py       │                                                  
│   PGLogger._runscript   │                                                  
│                         │                                                  
└─────────────────────────┘                                                  
```
~~~

### 继承自Bdb

```python
{{#include ../../../../../code_collections/python_tutor_service/pg_logger.py:502:502}}
```

### 初始化相关

```python
{{#include ../../../../../code_collections/python_tutor_service/pg_logger.py:511:522}}
```

### 调用函数

```python
{{#include ../../../../../code_collections/python_tutor_service/pg_logger.py:1668:1699}}
```

## 参考资料

- [KuanHsiaoKuo/OnlinePythonTutor](https://github.com/KuanHsiaoKuo/OnlinePythonTutor)
- [KuanHsiaoKuo/PythonTutorService: 基于OnlinePythonTutor开源项目进行改写](https://github.com/KuanHsiaoKuo/PythonTutorService)
- [bdb与pdb介绍](/layer1_underlying_abstract/3_language_grammar/code_quality.html)