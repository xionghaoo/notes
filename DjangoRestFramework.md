## 请求和响应

```python
request.data # 处理任意数据，支持'POST'、'PUT'、'PATCH'方法
request.POST # 处理表单数据，支持'POST'方法
```



`rest_framework`的装饰器`@api_view`保证视图可以接收到request实例，并且向`Response`对象中添加上下文以便提供内容协商。



GET请求可以通过改变accept头，或者添加`.json`等后缀来改变视图返回的内容格式。

POST请求提交的数据可以以表单或者json请求体的形式来提交，视图都可以正确解析。



## 基于类的视图

```python
class SnippetList(generics.ListCreateAPIView):
    # 设置返回的数据，不要直接访问这个属性，用get_queryset方法来获取
    queryset = Snippet.objects.all()
    # 数据序列化需要
    serializer_class = SnippetSerializer


class SnippetDetail(generics.RetrieveUpdateDestroyAPIView):
    queryset = Snippet.objects.all()
    serializer_class = SnippetSerializer
```

用通用的视图类可以写很简洁的代码



## MySql配置

1. 修改`settings.py`文件

```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'test',
        'HOST': '127.0.0.1',
        'PORT': 3306,
        'USER': 'root',
        'PASSWORD': '123456'
    }
}
```

2. 修改`settings.py`同级目录下的`__init__.py`文件

```python
import pymysql

pymysql.install_as_MySQLdb()
```





## 相关命令

根据Model生成数据库迁移（Migration）文件，创建或者更新数据库的必要文件，

```shell
$ python manage.py makemigrations snippets 
```

生成默认的数据库表

```shell
$ python .\manage.py migrate
```

生成Model的数据库表

```shell
$ python .\manage.py migrate snippets
```

创建管理员账号

```shell
$ python .\manage.py createsuperuser
```

