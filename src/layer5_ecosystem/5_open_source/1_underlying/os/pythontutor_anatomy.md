# pythontutor项目解剖

<!--ts-->
* [pythontutor项目解剖](#pythontutor项目解剖)
   * [说明](#说明)
   * [总体结构图](#总体结构图)
   * [iframe使用示例](#iframe使用示例)
   * [使用bottle写web服务](#使用bottle写web服务)
   * [pg_logger.py介绍](#pg_loggerpy介绍)
      * [继承自Bdb](#继承自bdb)
      * [初始化相关](#初始化相关)
      * [调用函数](#调用函数)
      * [返回数据示例](#返回数据示例)
   * [参考资料](#参考资料)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: kuanhsiaokuo, at: Fri Jul  1 10:38:44 CST 2022 -->

<!--te-->
## 说明

本文主要剖析一下pythontutor的项目结构与业务流程，并确定后面的相关代码可视化的使用方法

## 总体结构图

~~~admonish info title='pythontutor结构图'
```
                                                                                                                                                  ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓ 
                                                                                                                                                  ┃                              _                    _                            ┃ 
                                                                                                                                                  ┃                _ __  __ _ __| |____ _ __ _ ___   (_)___ ___ _ _                ┃ 
                                                                                                                                                  ┃               | '_ \/ _` / _| / / _` / _` / -_)_ | (_-</ _ \ ' \               ┃ 
                                                                                                                                                  ┃               | .__/\__,_\__|_\_\__,_\__, \___(_)/ /__/\___/_||_|              ┃ 
┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓                                                               ┃               |_|                    |___/     |__/                            ┃ 
┃       ___      _   _               _____     _             ___                  ┃                                                               ┃                                                                                ┃ 
┃      | _ \_  _| |_| |_  ___ _ _   |_   _|  _| |_ ___ _ _  | __|_ _____ __       ┃                                                               ┃   {                                                                            ┃ 
┃      |  _/ || |  _| ' \/ _ \ ' \    | || || |  _/ _ \ '_| | _|\ \ / -_) _|      ┃                                                               ┃     "name": "v5-unity",                                                        ┃ 
┃      |_|  \_, |\__|_||_\___/_||_|   |_| \_,_|\__\___/_|   |___/_\_\___\__|      ┃                                                               ┃     "version": "1.0.0",                                                        ┃ 
┃           |__/                                                                  ┃                                                               ┃     "description": "OPT v5",                                                   ┃ 
┃                                                                                 ┃                                                               ┃     "main": "bundle.js",                                                       ┃ 
┃                                                                                 ┃                                                               ┃     "repository": "https://github.com/pgbovine/OnlinePythonTutor",             ┃ 
┃                                                                                 ┃                                                               ┃     "license": "BSD-3-Clause",                                                 ┃ 
┃                                                                                 ┃                                                               ┃     "scripts": {                                                               ┃ 
┃  ┌────────────────────────┐                                                     ┃                                                               ┃       "start": "python bottle_server.py",                                      ┃ 
┃  │                        │                                                     ┃                                                               ┃       "py3start": "python3 bottle_server.py",                                  ┃ 
┃  │    botter_server.py    │                                                     ┃                                                     ┌─────────┫       "webpack": "webpack --devtool sourcemap --progress --colors --watch",    ┃ 
┃  │      get_py_exec       │────────────┐                                        ┃                                                     │         ┃       "production-build": "rm -f build/* && webpack && python                  ┃ 
┃  │                        │            │                                        ┃                                                     │         ┃   add_cache_busting_query_strings.py"                                          ┃ 
┃  └────────────────────────┘            │                                        ┃                                                     │         ┃     },                                                                         ┃ 
┃                                        │                                        ┃                                                     │         ┃     "dependencies": {                                                          ┃ 
┃                                        │           ┌─────────────────────────┐  ┃                                                     │         ┃       "css-loader": "",                                                        ┃ 
┃                                        └───────────▶                         │  ┃                                   npm run production-build    ┃       "fs-extra": "^6.0.1",                                                    ┃ 
┃                                                    │      pg_logger.py       │  ┃                                                     │         ┃       "json-stable-stringify": "^1.0.1",                                       ┃ 
┃                                                    │  exec_script_str_local  │  ┃                                                     │         ┃       "on-build-webpack": "^0.1.0",                                            ┃ 
┃                                        ┌───────────┤                         │  ┃                                                     │         ┃       "pixelmatch": "^4.0.2",                                                  ┃ 
┃                                        │           └─────────────────────────┘  ┃                  ┌─────────────────────────┐        │         ┃       "puppeteer": "^1.5.0",                                                   ┃ 
┃                                        │                                        ┃                  │                         │        │         ┃       "script-loader": "",                                                     ┃ 
┃                                        │                                        ┃                  │     /visualize.html     │        │         ┃       "style-loader": "",                                                      ┃ 
┃                                        │                       ┌────────────────╋──────────────────▶   visualize.bundle.js   ◀────────┘         ┃       "ts-loader": "^3.5.0",                                                   ┃ 
┃  ┌─────────────────────────┐           │                       │                ┃                  │                         │                  ┃       "url-loader": ""                                                         ┃ 
┃  │                         ◀───────────┘               return trace info        ┃                  └─────────────────────────┘                  ┃     }                                                                          ┃ 
┃  │      pg_logger.py       │                         {code: '', trace: []}      ┃                                                               ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━▲━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛ 
┃  │   PGLogger._runscript   │                                   │                ┃                                                                                                       │                                          
┃  │                         ├───────────────────────────────────┘                ┃                                                                                                       │                                          
┃  └─────────────────────────┘                                                    ┃                                                                                                   webpack                                        
┃                                                                                 ┃                                                                                                       │                                          
┃                                                                                 ┃                                                                                                       │                                          
┃                                                                                 ┃                                                                ┏━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┻━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┓
┃                                                                                 ┃                                                                ┃                  _                  _                  __ _         _          ┃
┃                                                                                 ┃                                                                ┃      __ __ _____| |__ _ __  __ _ __| |__  __ ___ _ _  / _(_)__ _   (_)___      ┃
┃                                                                                 ┃                                                                ┃      \ V  V / -_) '_ \ '_ \/ _` / _| / /_/ _/ _ \ ' \|  _| / _` |_ | (_-<      ┃
┃                                                                                 ┃                                                                ┃       \_/\_/\___|_.__/ .__/\__,_\__|_\_(_)__\___/_||_|_| |_\__, (_)/ /__/      ┃
┃                                                                                 ┃                                                                ┃                      |_|                                   |___/ |__/          ┃
┃                                                                                 ┃                                                                ┃   entry: {                                                                     ┃
┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛                                                                ┃       'visualize': "./js/visualize.ts",                                        ┃
                                                                                                                                                   ┃       'recorder': "./js/recorder.ts",                                          ┃
                                                                                                                                                   ┃       'render-trace': "./js/render-trace.ts",                                  ┃
                                                                                                                                                   ┃       'opt-live': "./js/opt-live.ts",                                          ┃
                                                                                                                                                   ┃       'iframe-embed': "./js/iframe-embed.ts",                                  ┃
                                                                                                                                                   ┃       'index': "./js/index.ts",                                                ┃
                                                                                                                                                   ┃       'composingprograms': "./js/composingprograms.ts",                        ┃
                                                                                                                                                   ┃       'csc108h': "./js/csc108h.ts",                                            ┃
                                                                                                                                                   ┃       'pytutor-embed': "./js/pytutor-embed.ts",                                ┃
                                                                                                                                                   ┃   },                                                                           ┃
                                                                                                                                                   ┃                                                                                ┃
                                                                                                                                                   ┃   output: {                                                                    ┃
                                                                                                                                                   ┃       path: __dirname + "/build/",                                             ┃
                                                                                                                                                   ┃       // TODO: use 'bundle.[hash].js' for fingerprint hashing                  ┃
                                                                                                                                                   ┃       // to create unique filenames for releases:                              ┃
                                                                                                                                                   ┃       // https://webpack.github.io/docs/long-term-caching.html                 ┃
                                                                                                                                                   ┃       filename: "[name].bundle.js",                                            ┃
                                                                                                                                                   ┃       sourceMapFilename: "[file].map",                                         ┃
                                                                                                                                                   ┃   },                                                                           ┃
                                                                                                                                                   ┗━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━┛
```
~~~

## iframe使用示例

<iframe width="950" height="500" frameborder="0" src="https://pythontutor.com/iframe-embed.html#code=%23%20The%20__init__%20'constructor'%20-%20object-oriented%20programming%20intro%0A%23%20Adapted%20from%20MIT%206.01%20course%20notes%20%28Section%203.5%29%0A%23%20http%3A//mit.edu/6.01/mercurial/spring10/www/handouts/readings.pdf%0A%0Aclass%20Staff601%3A%0A%20%20%20%20course%20%3D%20'6.01'%0A%20%20%20%20building%20%3D%2034%0A%20%20%20%20room%20%3D%20501%0A%0A%20%20%20%20def%20__init__%28self,%20name,%20role,%20years,%20salary%29%3A%0A%20%20%20%20%20%20self.name%20%3D%20name%0A%20%20%20%20%20%20self.role%20%3D%20role%0A%20%20%20%20%20%20self.age%20%3D%20years%0A%20%20%20%20%20%20self.salary%20%3D%20salary%0A%0A%20%20%20%20def%20salutation%28self%29%3A%0A%20%20%20%20%20%20%20%20return%20self.role%20%2B%20'%20'%20%2B%20self.name%0A%0Apat%20%3D%20Staff601%28'Pat',%20'Professor',%2060,%20100000%29%0Aprint%28pat.salutation%28%29%29&codeDivHeight=400&codeDivWidth=350&cumulative=false&curInstr=3&heapPrimitives=nevernest&origin=opt-frontend.js&py=3&rawInputLstJSON=%5B%5D&textReferences=false"> </iframe>

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

### 返回数据示例

```json lines
{{#include ../../../../../code_collections/python_tutor_service/pytutor_return_value_example.json:1:642}}
```

## 参考资料

- [KuanHsiaoKuo/OnlinePythonTutor](https://github.com/KuanHsiaoKuo/OnlinePythonTutor)
- [KuanHsiaoKuo/PythonTutorService: 基于OnlinePythonTutor开源项目进行改写](https://github.com/KuanHsiaoKuo/PythonTutorService)
- [bdb与pdb介绍](/layer1_underlying_abstract/3_language_grammar/code_quality.html)
- [Online Python Tutor- Embeddable Web-Based Program Visualization for CS Education.pdf](https://web.archive.org/web/20220701012126/http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.463.9294&rep=rep1&type=pdf)
    > [local dt](x-devonthink-item://6C44A26B-F75A-414F-830E-B8F96CD5A32B)
- [Online python tutor: embeddable web-based program visualization for cs education](https://www.connectedpapers.com/main/641cb912e58c54e8c8e1a741c2df8d5341a4a487/Online-python-tutor:-embeddable-web%20based-program-visualization-for-cs-education/graph)
- [Python Tutor code visualizer: Visualize code in Python, JavaScript, C, C++, and Java](https://pythontutor.com/visualize.html#mode=edit)