# 内置库介绍

<!--ts-->
* [内置库介绍](#内置库介绍)
   * [os库](#os库)
      * [环境变量设置及妙用](#环境变量设置及妙用)
* [参考资源](#参考资源)

<!-- Created by https://github.com/ekalinin/github-markdown-toc -->
<!-- Added by: runner, at: Wed Jul 20 03:35:21 UTC 2022 -->

<!--te-->

## os库

### 环境变量设置及妙用
> 本来打算在执行爬虫脚本时通过环境变量设置切换来半手动绕过拖块等验证码手段，但是Python只会在第一次运行时捕获环境变量，所以不现实。

```admonish quote title='官方文档对os.environ的介绍'
一个 mapping 对象，其中键值是代表进程环境的字符串。 例如，environ['HOME'] 是你的主目录（在某些平台上）的路径名，相当于 C 中的 getenv("HOME")。

> 这个映射是在第一次导入 os 模块时捕获的，通常作为 Python 启动时处理 site.py 的一部分。除了通过直接修改 os.environ 之外，在此之后对环境所做的更改不会反映在 os.environ 中。

该映射除了可以用于查询环境外，还能用于修改环境。当该映射被修改时，将自动调用 putenv()。

在Unix系统上，键和值会使用 sys.getfilesystemencoding() 和 'surrogateescape' 的错误处理。如果你想使用其他的编码，使用 environb。

> 注解 直接调用 putenv() 并不会影响 os.environ ，所以推荐直接修改 os.environ 。
> 注解 在某些平台上，包括 FreeBSD 和 macOS，设置 environ 可能导致内存泄漏。 请参阅 putenv() 的系统文档。
```

---

# 参考资源

- [os --- 多种操作系统接口 — Python 3.10.5 文档](https://docs.python.org/zh-cn/3/library/os.html)