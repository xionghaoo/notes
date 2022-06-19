## 命令

+ 创建app

```shell
$ python manage.py startapp demoapp
```



## 模型

+ `objects`：模型中最重要的属性是Manager，它是 Django 模型和数据库查询操作之间的接口，并且它被用作从数据库当中 获取实例。如果没有指定自定义的 `Manager` 默认名称是 [`objects`](https://docs.djangoproject.com/zh-hans/4.0/ref/models/class/#django.db.models.Model.objects)。Manager 只能通过模型类来访问，不能通过模型实例来访问。

### 字段

+ `choices`

```python
YEAR_IN_SCHOOL_CHOICES = [
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
]
```

第一个参数存储在数据库中，第二个参数用来在表单中显示

+ [`verbose_name`](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#django.db.models.Field.verbose_name)：字段备注名
+ [`primary_key`](https://docs.djangoproject.com/zh-hans/4.0/ref/models/fields/#django.db.models.Field.primary_key)：主键

#### Foreign Key

+ `blank`不会对数据库中体现，只在表单验证时有用
+ `null`会影响数据库

### 执行查询

Manager是Model的QuerySets主要来源

```python
>>> Entry.objects.filter(
...     headline__startswith='What'
... ).exclude(
...     pub_date__gte=datetime.date.today()
... ).filter(
...     pub_date__gte=datetime.date(2005, 1, 30)
... )
```

基本的查询关键字参数遵照 `field__lookuptype=value`，有双下划线.

`gte=`相当于sql中的`>=`，

`lte=`相当于`<=`

`exact=`相当于`=`，等同于不包含双下划线的情况

`iexact=`也是相当于`=`，且不区分大小写的匹配

`contains=`相当于`LIKE '%name%'`，大小写敏感的包含测试

`contains=`，不区分大小写的包含

### 模型方法

模型内置的方法可以被重写，比如`save`和`delete`方法，但是批量操作时不会生效，批量操作不会调用这些方法。

## 处理Http请求

`ETag`HTTP 响应头是资源的特定版本的标识符。这可以让缓存更高效，并节省带宽，因为如果内容没有改变，Web 服务器不需要发送完整的响应。而如果内容发生了变化，使用 ETag 有助于防止资源的同时更新相互覆盖（“空中碰撞”）。

### URL调度

+ `URLconf`：这个模块是纯粹的 Python 代码，包含 URL 模式（简单的正则表达式）到 Python 函数（你的视图）的简单映射。

```python
from django.urls import include, path
from . import views

urlpatterns = [
    path('<page_slug>-<page_id>/', include([
        path('history/', views.history),
        path('edit/', views.edit),
        path('discuss/', views.discuss),
        path('permissions/', views.permissions),
    ])),
]
```

### 视图装饰器

+ `@require_http_methods`控制允许的请求方法

```python
from django.views.decorators.http import require_http_methods

@require_http_methods(["GET", "POST"])
def my_view(request):
    # I can assume now that only GET or POST requests make it this far
    # ...
    pass
```

### 文件上传

```python
from django import forms

class UploadFileForm(forms.Form):
    title = forms.CharField(max_length=50)
    file = forms.FileField()
```

处理这个表单的视图将在 [`request.FILES`](https://docs.djangoproject.com/zh-hans/4.0/ref/request-response/#django.http.HttpRequest.FILES) 中接收文件数据，它是一个字典，包含表单中每个 [`FileField`](https://docs.djangoproject.com/zh-hans/4.0/ref/forms/fields/#django.forms.FileField) （或 [`ImageField`](https://docs.djangoproject.com/zh-hans/4.0/ref/forms/fields/#django.forms.ImageField)，或其他 [`FileField`](https://docs.djangoproject.com/zh-hans/4.0/ref/forms/fields/#django.forms.FileField) 子类）的键。所以上述表单中的数据将以 `request.FILES['file']` 的形式被访问。

注意 [`request.FILES`](https://docs.djangoproject.com/zh-hans/4.0/ref/request-response/#django.http.HttpRequest.FILES) 只有当请求方法是 `POST`，至少有一个文件字段被实际发布，并且发布请求的 `<form>` 有 `enctype="multipart/form-data"` 属性时，才会包含数据。否则 `request.FILES` 将为空。

### 中间件

```python
class SimpleMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response
        # One-time configuration and initialization.

    def __call__(self, request):
        # Code to be executed for each request before
        # the view (and later middleware) are called.

        response = self.get_response(request)

        # Code to be executed for each request/response after
        # the view is called.

        return response
```

接受一个`get_response`并返回一个中间件，返回的中间件可能是视图，也可能是中间件。

