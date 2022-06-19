+ `WSGI`

Web服务器网关接口（Python Web Server Gateway Interface，缩写为WSGI）是为Python语言定义的Web服务器和Web应用程序或框架之间的一种简单而通用的接口

- [Werkzeug](https://palletsprojects.com/p/werkzeug/) 用于实现 WSGI ，应用和服务之间的标准 Python 接口。[WSGI 是什么，看完一定懂](https://foofish.net/python-wsgi.html)
- `Nginx`：[8分钟带你深入浅出搞懂Nginx](https://zhuanlan.zhihu.com/p/34943332)

## 处理请求

```python
@app.route('/path/<path:subpath>')
def show_subpath(subpath):
    # show the subpath after /path/
    return f'Subpath {escape(subpath)}'
```

转换器支持：

| `string` | （缺省值） 接受任何不包含斜杠的文本 |
| -------- | ----------------------------------- |
| `int`    | 接受正整数                          |
| `float`  | 接受正浮点数                        |
| `path`   | 类似 `string` ，但可以包含斜杠      |
| `uuid`   | 接受 UUID 字符串                    |

#### 响应

响应自动转换规则：

1. 如果视图返回的是一个响应对象，那么就直接返回它。
2. 如果返回的是一个字符串，那么根据这个字符串和缺省参数生成一个用于返回的 响应对象。
3. 如果返回的是一个字典，那么调用 `jsonify` 创建一个响应对象。
4. 如果返回的是一个元组，那么元组中的项目可以提供额外的信息。元组中必须至少 包含一个项目，且项目应当由 `(response, status)` 、 `(response, headers)` 或者 `(response, status, headers)` 组成。 `status` 的值会重载状态代码， `headers` 是一个由额外头部值组成的列表 或字典。
5. 如果以上都不是，那么 Flask 会假定返回值是一个有效的 WSGI 应用并把它转换为 一个响应对象。

如果想要在视图内部掌控响应对象的结果，那么可以使用 [`make_response()`](https://dormousehole.readthedocs.io/en/latest/api.html#flask.make_response) 函数。

## 数据库

[`g`](https://dormousehole.readthedocs.io/en/latest/api.html#flask.g) 是一个特殊对象，独立于每一个请求。在处理请求过程中，它可以用于储存 可能多个函数都会用到的数据。把连接储存于其中，可以多次使用，而不用在同一个 请求中每次调用 `get_db` 时都创建一个新的连接。

[`current_app`](https://dormousehole.readthedocs.io/en/latest/api.html#flask.current_app) 是另一个特殊对象，该对象指向处理请求的 Flask 应用