# Django Migrate
작성한 models.py 파일을 사용하여 table을 생성합니다  
```
> python manage.py makemigrations hello_visitor
> python manage.py sqlmigrate hello_visitor 0011
> python manage.py migrate hello_visitor
> python manage.py showmigrations hello_visitor
```
각 명령은 app별로 실행합니다  
app명을 주지 않으면 전체 프로젝트에 대하여 table을 생성합니다  
makemigrations은 migrate를 위한 메타파일을 생성합니다  
작업파일은 migration 폴더아래 순차적으로 파일이 생성됩니다
migrate 문은 실제 db 테이블에 작업을 실행합니다
sqlmigrate 문을 실행하여 작업할 ddl문을 확인할 수 있습니다  
sqlmigrate 문은 생략가능합니다
migration folder의 파일을 참조하여 변경내용을 인식하므로 다음작업을 위하여 보관이 필요합니다


https://docs.djangoproject.com/ko/2.2/topics/migrations/
