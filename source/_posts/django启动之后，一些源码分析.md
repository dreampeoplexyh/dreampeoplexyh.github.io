---
title: django启动之后，关于View源码分析
date: 2019-03-30 17:55:46
tags:
categories: Django
---


![](django启动之后，一些源码分析\14.png)

<!--more-->

django启动之后，我们可以看看他的逻辑顺序

![](django启动之后，一些源码分析\1.png)

drf view的基本思路
![da24807c038420307ae200feabcb8d49.png](django启动之后，一些源码分析\2.png)

我们先来了解下结构关系，大体的结构关系就是如图所示
view-->APIView-->GenericAPIView-->GenericViewSet(viewsets)

django的CBV和FBV
1. FBV（function base views）就是在视图里使用函数处理请求
2. CBV（class base views）就是在视图中使用类处理请求


就好比我们要处理一个GET方法的view,用函数写就这样
```
from django.http import HttpResponse

def my_view(request):
    if request.method == 'GET':
        return HttpResponse('ok')
```
如果用类方法我们可以这样

```
from django.http import HttpResponse
from django.views import View
  
class MyView(View):
      def get(self, request):
            return HttpResponse('OK')
```



#### View源码解析
在View类中 的类方法as_view,放回了一个view.function对象

![4bfad9b958be88419f7949c355a7f4b8.png](django启动之后，一些源码分析\3.png)
![91d826112f73f430bbec57e074afe0be.png](django启动之后，一些源码分析\4.png)


![5e0914dfbfdbec41a577273728643513.png](django启动之后，一些源码分析\5.png)

![aa956b930576d5c350b2ad2e8ac804aa.png](django启动之后，一些源码分析\6.png)

当我们启动的时候
![91d826112f73f430bbec57e074afe0be.png](django启动之后，一些源码分析\7.png)


如果重写dispatch方法，就会走我们自己写的dispatch

```
from django.views import  View
class IndexView(View):
    def get(self,request, *args, **kwargs):
        print("get")
        return HttpResponse("ok")

    def dispatch(self, request, *args, **kwargs):
        print("dispatch")
        ret = super(IndexView,self).dispatch(request, *args, **kwargs)
        print("ret",ret )
        return  HttpResponse(ret)
```


流程如下

print("dispatch") ------> print("get") ---------> return HttpResponse("ok") -------> print("ret",ret ) -----> return HttpResponse(ret)

总结： 从View 的源码中可以分析出，执行View的 as_view方法意味着要去执行当前调用对象的dispatch方法
#### APIView（rest_framework.views.APIView）源码解析
我们先来明确下APIView是干什么的，他是APIView是REST framework提供的所有视图的基类，继承Django View父类

APIView与view的不同之处在于

1. 传入到视图方法中的是REST framework的Request对象，而不是Django的HttpRequest对象
2. 视图方法可以放回REST framework的Response对象，视图会为响应数据设置（render）符合前端要求的格式
3. 任何APIException异常都会被捕获到，并且处理成合适的响应信息
4. 在进行dispatch()分发前，会对请求进行身份认证、权限检查、流量控制

支持定义的属性

1. authentication_classes 列表或元祖，身份认证类
2. permissoin_classes列表或元祖，权限检查类
3. throttle_classes列表或元祖，流量控制类

APIView方法有很多方法，我们先来看看他的as_view方法
![db83c5e5f02e497c4e3dfcfadb5b5310.png](django启动之后，一些源码分析\8.png)

从他的super()方法中我们可以知道，他是去执行APIView类的as_view方法，最后执行当前调用对象的dispatch方法（**参考上面总结**），在APIView 他重写了他的父类（View）的dispath方法
![17c7a41743efc826bf837d0a24623770.png](django启动之后，一些源码分析\9.png)

我们可以点进去initialize_request中看看到底添加了什么功能
![52d8366131fdefeb84f3b65916428716.png](django启动之后，一些源码分析\10.png)

initialize_request 最后返回了一个Request对象，该对象接受了自定义CBV的request,我们再来看看这个Request对象进行了什么的处理

Request对象
![fa68f740b13c5926caaa5a7c54cf4d0f.png](django启动之后，一些源码分析\11.png)

![23dcb3e8cd12b7dc3e7d587e4cc892ad.png](django启动之后，一些源码分析\12.png)

1. request.query_params        可以拿到原来URL里面的参数
2. request.data                    可以拿到前端提交过来的所有数据

总结：源码解析
![75a041cd897c4c19848e018ab9378cbd.png](django启动之后，一些源码分析\13.png)

* * *


#### GenericAPIView(rest_framework.generics.GenericAPIView)
他是继承自APIView,增加对与列表视图和详情视图可能用到的通用支持方法，通常使用时，可以搭配一个或多个Mixin扩展类

>支持定义的属性

1. 列表视图与详情视图的查询集
        queryset 列表视图的查询集
        serializer_class 视图使用的序列化器
        
 1. 列表视图的使用
        pagination_class分页控制类
        filter_backends 过滤控制后端
       
 2. 详情页视图的使用
        lookup_field 查询单一数据库对像时使用的条件字段， 默认为‘pk’
        lookup_url_kwarg 查询单一数据时URL中的参数关键字名称，默认与look_field相同
        
> 提供的方法  

1. 列表视图与详情视图通用

* get_querset(self)
   返回视图使用的查询集，是列表视图与详情视图获取数据的基础 ，默认返回queryset属性，可以重写，例如：

```
def get_querset(self):
    user = self.request.user
    return user.accounts.all()
```

* get_serializer_class(self)
   返回序列化类，默认返回serializer_class, 可以重写，例如：
   
  
```
 def get_serializer_class(serlf):
        if self.request.user.is_staff:
            return FullAccountSerializer
        return BasicAccountSerializer
```
  
>  get_serializer(self, args, *kwargs)
  

*   返回序列对象，被其他视图或扩展类使用，如果我在视图中想要获取序列化器对象，可以直接调用此方法
*   注意，在提供序列化器的时候，REST framework会向对象的context属性补充三个数据：reuest, format, view, 这三个数据对象可以在定义序列化器时使用

>详情视图使用：
 

*   get_object(self) 返回详情视图所需的模型类数据对象，默认使用lookup_field参数来过滤queryset。在视图中可以调用改方法获取详情信息的模型类对象
*   若详情访问的模型类对象不存在，会返回404。
*   该方法会默认使用APIView提供的check_object_permissions方法检查当前对象是否有权限访问

```
  class BookDetailView(GenericAPIView)；
        queryset = BookInfoSerializer
        
        def get(self, request, pk):
               book = self.get_object()
               serializer = self.get_serializer(book)
               return Response(serializer.data)
```
    
    
    
####  五个扩展类

 ListModelMixin
 列表视图扩展类，提供list(request, *args, **kwargs)方法快速实现列表视图，返回200状态码。
 
 该Mixin的list方法会对数据进行过滤和分页
 源代码：
 
     
```
class ListModelMixin(object):
    """
    List a queryset.
    """
    def list(self, request, *args, **kwargs):
        # 过滤
        queryset = self.filter_queryset(self.get_queryset())
        # 分页
        page = self.paginate_queryset(queryset)
        if page is not None:
            serializer = self.get_serializer(page, many=True)
            return self.get_paginated_response(serializer.data)
        # 序列化
        serializer = self.get_serializer(queryset, many=True)
```



举例：
```
from rest_framework.mixins import ListModelMixin
class BookListView(ListModelMixin, GenericAPIView):
        queryset = BookInfo.object.all()
        serializer_class = BookInfoSerializer
        
        def get(self, request):
                return self.list(reqiest)
```

>CreateModelMixin

* 创建视图扩展类，提供create(reqiest, *args, **kwargs) 方法快速实现创建资源的视图， 成功返回201状态码

* 如果序列化器对前端发送的数据验证失败， 返回400错误

源代码：

```
class CreateModelMixin(object):
    """
    Create a model instance.
    """
    def create(self, request, *args, **kwargs):
        # 获取序列化器
        serializer = self.get_serializer(data=request.data)
        # 验证
        serializer.is_valid(raise_exception=True)
        # 保存
        self.perform_create(serializer)
        headers = self.get_success_headers(serializer.data)
        return Response(serializer.data, status=status.HTTP_201_CREATED, headers=headers)
 
    def perform_create(self, serializer):
        serializer.save()
 
    def get_success_headers(self, data):
        try:
            return {'Location': str(data[api_settings.URL_FIELD_NAME])}
        except (TypeError, KeyError):

```

>RetrieveModelMixin

详情视图扩展类， 提供retrieve(request, *args, **kwargs)方法，可以快速实现返回一个存在的数据对象

如果存在，返回200， 否则返回404.

```
class RetrieveModelMixin(object):
    """
    Retrieve a model instance.
    """
    def retrieve(self, request, *args, **kwargs):
        # 获取对象，会检查对象的权限
        instance = self.get_object()
        # 序列化
        serializer = self.get_serializer(instance)
        return Response(serializer.data)
```
举个例子


```
class BookDetailView(RetrieveModelMixin, GenericAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
 
    def get(self, request, pk):
        return self.retrieve(request)
```

举例


```
class BookDetailView(RetrieveModelMixin, GenericAPIView):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
 
    def get(self, request, pk):
        return self.retrieve(request)
```


>UpdateModelMixin


更新视图扩展类， 提供update(request, *args, **kwargs)方法，可以快速实现更新一个存在的数据对象

同事也提供partial_update(request, *args, **kwargs)方法， 可以实现局部更新
成功返回200， 序列化器校验数据失败时，返回400错误

源代码
```
class UpdateModelMixin(object):
    """
    Update a model instance.
    """
    def update(self, request, *args, **kwargs):
        partial = kwargs.pop('partial', False)
        instance = self.get_object()
        serializer = self.get_serializer(instance, data=request.data, partial=partial)
        serializer.is_valid(raise_exception=True)
        self.perform_update(serializer)

        if getattr(instance, '_prefetched_objects_cache', None):
            # If 'prefetch_related' has been applied to a queryset, we need to
            # forcibly invalidate the prefetch cache on the instance.
            instance._prefetched_objects_cache = {}

        return Response(serializer.data)

    def perform_update(self, serializer):
        serializer.save()

    def partial_update(self, request, *args, **kwargs):
        kwargs['partial'] = True
        return self.update(request, *args, **kwargs)
```

>DestroyModelMixin


删除视图扩展类，提供destroy（request, *args, **kwargs）方法，可以快速实现删除一个存在的数据对象

成功返回204， 不存在返回404

源代码：




```
class DestroyModelMixin(object):
    """
    Destroy a model instance.
    """
    def destroy(self, request, *args, **kwargs):
        instance = self.get_object()
        self.perform_destroy(instance)
        return Response(status=status.HTTP_204_NO_CONTENT)
 
    def perform_destroy(self, instance):
        instance.delet

```

其他的一些之类视图

1. CreateAPIView   
提供pist 方法
继承自：GenericAPIView、CreateModelMixin

2. ListAPIView
提供get方法·
继承自：GenericAPIView、ListModelMixin

3. RetireveAPIView
提供get方法
继承自： GenericAPIView、 RetrueveModelMixin

4. DestoryAPIView
提供delete方法
继承自：GenericAPIView、DestoryModelMixin

5. UpdateAPIView
提供put 和 patch方法
继承自：GenericAPIView、 UPdateAPIView

6. RetrieveUpdateAPIView
提供get、put、patch方法
继承自：GenericAPIView、RetrieveModelMixin、UpdateModelMixin

7. RetrieveUpdateDestoryAPIView
提供 get、put 、patch、delete方法
继承自：GenericAPIView、RetrieveModelMixin、UpdateModelMixin、DestoryModelMixin



#### 视图集

使用视图集ViewSet, 可以将一系列逻辑先关的工作放到一个类中：

1. list()提供一组数据
2. retrieve()提供单个数据
3. create()创建视图
4. update()保存数据
5. destory()删除数据

ViewSet视图集不在实现get()、post()等方法，而是实现动作action 如list()、create()等

视图集旨在使用as_view()方法的时候，才会将action动作与具体请求方式对应上。如

```
class BookInfoViewSet(viewsets.ViewSet):
        def list(self, request):
        ....
        
        def retrieve(self, request, pk=None):
        ......
        
       
```

我们在设置路由时，我们可以如下操作


```
urlpatterns = [
    url(r'^books/$', BookInfoViewSet.as_view({'get':'list'}),
    url(r'^books/(?P<pk>\d+)/$', BookInfoViewSet.as_view({'get': 'retrieve'})
]
```

action属性

我们在视图集中，我们可以通过action对象属性来获取当前请求视图集时的action动作那个

例如：
```
def get_serializer_class(self):
    if self.action == 'create':
        return OrderCommitSerializer
    else:
        return OrderDataSerializer
```

常见视图集父类

* ViewSet

继承自APIView，作用也与APIView基本类似，提供了身份认证，权限校验，流量管理等

在ViewSet中，没有提供动作action方法，需要我们自己实现action方法


* GenericViewSet

继承自GenericAPIView, 作用也与GennericAPIView类似， 提供了get_object、get_queryset等方法便于列表视图与详情信息视图的开发

* ModelViewSet

继承自GenericAPIView, 同时包括了ListModelMixin、RetrieveModelMixin、CreateModelMixin、UpdateModelMinxin、DestoryModelMixin


* ReadOnlyModelViewSet
继承自GenericAPIView, 同时包括了ListModelMixin、RetrieveModelMixin


视图集中定义附加action动作
在视图集中，出了上述默认的方法动作外，还可以添加自定义动作
添加自定义动作需要使用rest_framework.decorators.action装饰器
以action装饰器装饰的方法名会作为action动作名，与list、retrieve等同

action装饰器可以接收两个参数

* methods: 该action支持的请求方式，列表传递
* detail:表示是action中要处理的是否是视图资源的对象（即是否通过url路径获取主键）

    *           True 表示使用通过URL获取的主键对应的数据对象
    *           False 表示不适用URL获取主键

          
举个例子：

```

from rest_framework import mixins
from rest_framework.viewsets import GenericViewSet
from rest_framework.decorators import action
 
class BookInfoViewSet(mixins.ListModelMixin, mixins.RetrieveModelMixin, GenericViewSet):
    queryset = BookInfo.objects.all()
    serializer_class = BookInfoSerializer
 
    # detail为False 表示不需要处理具体的BookInfo对象
    @action(methods=['get'], detail=False)
    def latest(self, request):
        """
        返回最新的图书信息
        """
        book = BookInfo.objects.latest('id')
        serializer = self.get_serializer(book)
        return Response(serializer.data)
 
    # detail为True，表示要处理具体与pk主键对应的BookInfo对象
    @action(methods=['put'], detail=True)
    def read(self, request, pk):
        """
        修改图书的阅读量数据
        """
        book = self.get_object()
        book.bread = request.data.get('read')
        book.save()
        serializer = self.get_serializer(book)

```

url 的定义

   
```
urlpatterns = [
    url(r'^books/$', views.BookInfoViewSet.as_view({'get': 'list'})),
    url(r'^books/latest/$', views.BookInfoViewSet.as_view({'get': 'latest'})),
    url(r'^books/(?P<pk>\d+)/$', views.BookInfoViewSet.as_view({'get': 'retrieve'})),
    url(r'^books/(?P<pk>\d+)/read/$', views.BookInfoViewSet.as_view({'put': 'read'})),
]

```
**


#### 总结

这次我们浅析了django到drf的一些源码，drf是如何对代码进行封装的，这在我们下次写代码时很有帮助。




