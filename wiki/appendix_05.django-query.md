# Django query
## query
Visit 모델객체의 전체 데이터를 조회합니다
```python
qs = Visit.objects.all()
```
Visit 모델객체에서 delete_yn='N'인 값을 조회합니다
```python
qs = Visit.objects.filter(delete_yn='N')
```
해당 query set의 count를 조회합니다
```python
tot_cnt = qs.count()
```
해당 query set을 visit_id의 역순으로 조회합니다
```python
qs = qs.order_by('-visit_id')
```
해당 query set을 offet부터 limit갯수만큼 paging(slicing)합니다
```python
qs = qs[offset:limit]
```

filter문에서 다음과 같은 조건을 사용할 수 있습니다
```python
. and 조건
  queryset = 모델명.queryset.all()
  queryset = queryset.filter(필드1=값1, 필드2=값2)
  queryset.filter(Q(필드1=값1) & Q(필드2=값2))
. or 조건
  queryset.filter(Q(필드1=값1) | Q(필드2=값2))
. 비교조건
  필드명__lt = 값   (필드명 < 값)
  필드명__lte = 값  (필드명 <= 값_
  필드명__gt = 값   (필드명 > 값)
  필드명__startswith = 값  (필드명 like "값%")
  필드명__endswith = 값    (필드명 like "%값")
  필드명__contains = 값    (필드명 like "%값%")
```

다음과 같은 방법으로 실행 sql문을 조회할수 있습니다
```
# queryset의 sql문 표시
qs.query

# 실행한 sql문을 index로 선택하여 조회할 수 있습니다
from django.db import connection
. . .
connection.queries[-1]
```

query 관련하여 상세한 내용은 다음 url을 참조합니다  
https://docs.djangoproject.com/en/2.2/topics/db/queries/

## relationships

Visit 객체와 relationship이 있는 VisitUser 객체를 조회합니다
```
visit = Visit.objects.get(visit_id=pk)
vu_list = visit.visituser_set.all()
```

추가 예제
```python
# VisitCar 객체의 visit_car_gate='E' 인 Visit 객체 조회
qs = Visit.objects.filter(visitcar__visit_car_gate='E')
print(qs.first().visit_id)
print(connection.queries[-1])
``` 
```
sql문 =>
SELECT "TBL_VISIT"."visit_id", ...
FROM "TBL_VISIT" INNER JOIN "TBL_VISIT_CAR" ON 
("TBL_VISIT"."visit_id" = "TBL_VISIT_CAR"."VISIT_ID") 
WHERE "TBL_VISIT_CAR"."visit_car_gate" = \'E\' ...
```
```python
# Visit 객체의 visit_id=1 인 VisitCar 객체 조회
qs = VisitCar.objects.filter(visit__visit_id=1)
print(qs.first().visit_car_id)
print(connection.queries[-1])
```
```
sql문 =>
SELECT "TBL_VISIT_CAR"."visit_car_id", ...
FROM "TBL_VISIT_CAR" 
WHERE "TBL_VISIT_CAR"."VISIT_ID" = 1 ...
```
```python
# visit_car_id=1 인 VisitCar 객체에서 Visit 객체를 참조
visitCar = VisitCar.objects.get(visit_car_id=1)
print(visitCar.visit.visit_id)
print(connection.queries[-2:-1])
```
```
sql문 =>
SELECT "TBL_VISIT_CAR"."visit_car_id", ...
FROM "TBL_VISIT_CAR" 
WHERE "TBL_VISIT_CAR"."visit_car_id" = 1

SELECT "TBL_VISIT"."visit_id", ...
FROM "TBL_VISIT" 
WHERE "TBL_VISIT"."visit_id" = 1'
```
```python
# visit_car_id=1 인 VisitCar 객체에서 Visit 객체를 참조(join)
visitCar = VisitCar.objects.select_related().get(visit_car_id=1)
print(visitCar.visit.visit_id)
print(connection.queries[-1])
```
```
sql문 =>
SELECT "TBL_VISIT_CAR"."visit_car_id", ...
"TBL_VISIT"."visit_id", ...
FROM "TBL_VISIT_CAR" INNER JOIN "TBL_VISIT" ON 
("TBL_VISIT_CAR"."VISIT_ID" = "TBL_VISIT"."visit_id") 
WHERE "TBL_VISIT_CAR"."visit_car_id" = 1'
```

python shell을 사용하여 간단히 query문을 실행하여 결과를 조회할 수 있습니다
```
> python manage.py shell
>>> from hello_visitor.models import Visit
>>> v = Visit.objects.first()
>>> v.visit_id
1
>>>
```
relationships query 관련하여 상세한 내용은 다음 url을 참조합니다
https://docs.djangoproject.com/en/2.2/topics/db/queries/#related-objects


## save
Visit 객체 data를 create합니다
```
visit = Visit()
visit.visit_request_user_id = user_id
...
visit.save()
```

Visit 객체 data를 update 합니다
```
visit = Visit.objects.get(visit_id=pk)
visit.visit_status_id = status_id
visit.save()
```

## delete
Visit 객체 data를 삭제합니다
```
visit = Visit.objects.get(visit_id=pk)
visit.delete()
```

## native sql
다음과 같은 형식으로 native sql문을 사용할 수 있습니다
```
ex)
    sql_stmt = '''
    SELECT V.VISIT_ID
        , V.VISIT_REQUEST_USER_ID
        . . .
    FROM TB_VISIT V
    WHERE DELETE_YN = 'N'
    AND V.VISIT_REQUEST_USER_ID = %s
    '''
        
    args = [visit_request_user_id]
    . . .

	with connection.cursor() as cursor:
    	cursor.execute(sql, args)
        visit_list = []  
        for row in cursor.fetchall():
        	visit_dic    = {
            	'visitId': row[0],
                'visitRequestUserId' : row[1],
                . . .
            }
            visit_list.append(visit_dic)

	return visit_list
```

## extra
```
ex)
qs = Visit.objects.extra(
    select={'code_name': 'default_name'}, 
    tables=['tbl_code_detail'], 
    where=['tbl_code_detail.group_id=7', 'code_key=visit_status_id'] )
    print(qs.first().code_name)
```

## transaction
기본적으로 ORM이 모든 쿼리를 호출할 때마다 자동으로 커밋을 하게 됩니다.  
### 각각의 HTTP요청을 트랜잭션으로 처리하려면..
Setting.py의 DATABASES 옵션에 ATOMIC_REQUEST 설정을 통해 모든 웹 요청을 쉽게 트랜잭션 처리할 수 있습니다.
```python
DATABASE = {
    'default':{
        #....
        'ATOMIC_REQEUSTS': True,
    }
}
```
ATOMIC_REQUEST 설정은 에러가 발생할 때 데이터베이스가 롤백됩니다.  
좀더 명시적으로 선언을 통해 transaction을 설정할 수 있습니다.  
```python
from django.db import transaction
#...

class VisitCreateCar(APIView):
	@transaction.atomic
	def post(self, request):
```

function에도 transaction을 설정할수 있습니다
```python
@transaction.atomic
def viewfunc(request):
    # This code executes inside a transaction.
    do_stuff()
```

settings.py파일에 전체 database에 대하여 transaction을 설정하는 방법도 있습니다  

상세한 내용은 다음 url을 참조합니다
[https://docs.djangoproject.com/en/2.2/topics/db/transactions/]

## connection
django framework은 PostgreSQL, SQLite, MySQL, Oracle등의 database를 지원합니다
다음은 settings.py에서 database 설정예입니다  
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': os.path.join(BASE_DIR, 'db.sqlite3'),
    }
}
```
django framework은 "Persistent connections"이라는 db접속방식을 사용합니다  
이 방식에서는 CONN_MAX_AGE라는 값을 사용합니다  

상세한 내용은 다음 url을 참조합니다
[https://docs.djangoproject.com/en/2.2/ref/databases/]
