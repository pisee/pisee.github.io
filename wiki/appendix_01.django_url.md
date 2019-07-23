# Django URL
## url 정의
보통 urls.py 파일에 web request url을 정의합니다  
아래의 예는 'api/v1/visits/history'라는 url을 요청하면  
visit_views.py 파일에 있는 VisitList class의 as_view를 호출합니다  

```
urlpatterns = [
    path('api/v1/visits/history', visit_views.VisitList.as_view()),
    path('api/v1/visits/approval', visit_views.VisitList.as_view()),
    path('api/v1/visits', visit_views.VisitList.as_view()),
    . . .
]
```

project(예:python_template_web) urls.py파일 에 app별 url파일을 정의해야합니다
```
urlpatterns = [
    path('admin/', admin.site.urls),
    path('admin', admin.site.urls),

    path('', include('hello_visitor.urls')),
    path('', include('hello_common.urls')),
    path('swagger-ui', schema_view),
]
```