# Django Model

## model
모델이란 부가적인 메타데이터를 가진 데이터베이스의 구조(layout)를 말합니다.

### model 작성
보통 app 생성시 기본으로 생성된 models.py 파일에 작성합니다  
다음은 model class 작성예입니다  

```
from django.db import models

class Visit(models.Model):
    visit_id = models.AutoField(primary_key=True)
    visit_status_id = models.BigIntegerField(default=1)
    visit_request_user_full_name = models.CharField(max_length=100)
    visit_start_date = models.DateTimeField(auto_now=True)
    visit_purpose = models.CharField(max_length=100, null=True)
    . . .
    class Meta:
        db_table = ('TBL_VISIT')
```

모델 클래스는 models.Model을 상속하여 작성합니다  
모델의 필드들을 작성합니다  
다음은 model class에서 사용할수 있는 field type, option 들 입니다
```
- model fied type
. primary key : AutoField
. 문자열 : CharField, TextField, SlugField
. 숫자 : IntegerField, BigIntegerField, DecimalField, FloatField 등
. 날짜/시간 : DateField, DateTimeField, TimeField

- model fied option
. primary_key=True
. null=True
. default=1
. max_length=100
. unique=True
. db_column

- meta class : 모델 클래스 전체에 속성을 적용합니다
. example
  class Meta:
    db_table = ('TBL_VISIT')
    ordering = ["-visit_id"]
```

모델작성에 필요한 상세한 문법은 다음 url을 참고합니다  
[https://docs.djangoproject.com/en/2.2/topics/db/models/]

### relationships
Visit와 VisitUser 객체가 1대 N일때 아래와 같이 ForeignKey로 relationships을 정의합니다
```
class VisitUser(models.Model):
    visit_user_id = models.AutoField(primary_key=True)
    visit = models.ForeignKey(Visit, on_delete=models.CASCADE, db_column='VISIT_ID')
    visit_user_full_name = models.CharField(max_length=50)
    . . .
```

1:1 관계는 OneToOneField로 정의합니다  
```
ex)
class EntryDetail(models.Model):
    entry = models.OneToOneField(Entry, on_delete=models.CASCADE)
    details = models.TextField()
```

