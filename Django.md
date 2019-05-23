## Django

- install 
```
$ pip install Django
```
- version
```
$ python -m django --version
```
- create project
```
$ django-admin startproject pj_demo

# directory
 
 project
│   
└───pj_demo
    │   __init__.py
    |   settings.py
    │   urls.py
    |   wsgi.py
```

- run server
```
$ python manage.py runserver
```
- create app demo
```
$ python manage.py startapp polls

# directory
project   
│
└───pj_demo
|    │   __init__.py
|    |   settings.py
|    │   urls.py
|    |   wsgi.py
|
└───polls
    │   __init__.py
    |   admin.py
    |   apps.py
    |   models.py
    |   tests.py
    |   urls.py
    |   views.py

```

- 为了在我们的工程中包含这个应用，我们需要在配置类 INSTALLED_APPS 中添加设置。
  因为 PollsConfig 类写在文件 polls/apps.py 中，所以它的点式路径是 'polls.apps.PollsConfig'。在文件 pj_demo/settings.py 中 INSTALLED_APPS 子项添加点式路径后，它看起来像这样：
```
# Application definition

INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]
  ```
  
 - model
 ```
 # 为模型的改变生成迁移文件。
 $ python manage.py makemigrations 
 
 # 自动执行数据库迁移并同步管理你的数据库结构
 $ python manage.py migrate
 
 $ python manage.py createsuperuser
```

### rest api

- install 
```
$ pip install djangorestframework
```
- start app
```
$ python manage.py startapp rest_demo
```
- Add 'rest_framework' to your INSTALLED_APPS setting.
```
INSTALLED_APPS = (
    ...
    'rest_framework',
)
```

- Add the following to your root urls.py file.
```
urlpatterns = [
    ...
    path('rest/', include('rest_demo.urls'))
]
```

- view demo
```
# views.py

from rest_framework import status
from rest_framework.decorators import api_view
from rest_framework.response import Response
from rest_framework.decorators import APIView
from django.http import JsonResponse

# Create your views here.


@api_view(['get'])
def test_api(request):
    return Response("haaa", status=status.HTTP_200_OK)


class TestAPIView(APIView):
    def get(self, request):
        res = {'key': 'value'}
        return JsonResponse(res)
```
```
# urls.py

from django.conf.urls import url
from rest_demo import views
from rest_demo.views import TestAPIView

urlpatterns = [
    url('test_api', views.test_api),
    url('api', TestAPIView.as_view())
]
```


