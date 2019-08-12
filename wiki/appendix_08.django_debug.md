# Django Debug  

## django-extension
1. 설치
    ```
    $ pip install django-extension
    ```
    ```
    $ pip install Werkzeug
    ```
1. 설정
    ```python
    # setting.py
    INSTALLED_APPS = [
        ...
        'django_extensions',
    ]
    ```
1. django shell_plus 활용
    ```
    $ python manage.py shell_plus
    ```
1. runserver_plus 활용
    ```
    $ python manage.py runserver_plus
    ```

## django-debug-toolbar
1. 설치
    ```
    pip install django-debug-toolbar
    ```
1. 설정
    ```python
    # setting.py
    INSTALLED_APPS = [
        ...
        'debug_toolbar',
    ]

    MIDDLEWARE = [
        ...
        'django.contrib.staticfiles',
        ...
        'debug_toolbar.middleware.DebugToolbarMiddleware', 
    ]

    STATIC_URL = '/static/'
    INTERNAL_IPS = ('127.0.0.1',)
    ```
    ```python
    # urls.py
    from django.contrib import admin
    from django.urls import path, include
    from django.conf import settings


    urlpatterns = [
        path('admin/', admin.site.urls),
    ]

    if settings.DEBUG:
        import debug_toolbar
        urlpatterns += [
            path('__debug__/', include(debug_toolbar.urls)),
        ]
    ```
1. Django Template  활용시 활용할 수 있는 다양한 디버깅 기능을 제공합니다.
