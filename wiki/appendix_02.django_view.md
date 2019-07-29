# Django View
## view 작성

### class based view
아래 class는 django 기반 rest api view 작성예입니다  

```python
# ex)
class VisitList(APIView):
    def get(self, request):
        resp_dict = self.list_visit(request.GET)
        #print('# resp : ', resp_dict)
        return Response(resp_dict)

    def post(self, request):
        print('# request data visit : ', request.data) #dictionary
        v_ser = self.create_visit(request.data)
        print('# visit_id ', v_ser.data['visit_id'])
        
        return Response({"visitId": v_ser.data['visit_id']})
        
    def list_visit(self, data):
        visit_request_user_id = data.get('visitRequestUserId', '') #신청자
        . . .
        qs = Visit.objects.filter(delete_yn='N')
        . . .
        visit_list = []
        for row in qs:
            row_dict = VisitSerializer(row).data 	#serializer : model object => dict
            . . .
            visit_list.append(row_dict)
            
        resp_dict = {'content': visit_list, 'totalPages' : tot_page, ...}
        return resp_dict
```

django rest_framework의 APIView를 상속하여 class를 작성합니다  
```python
from rest_framework.views import APIView
. . .
class VisitList(APIView):
```

client에서 호출한 METHOD(get/post/put/delete) 에 따라  
각각 class view의 get/post/put/delete method가 실행됩니다 
```python
def get(self, request):
def post(self, request):
def put(self, request, pk, status_id):
def delete(self, request, pk):
```

get method의 경우 request.GET(dictionary data type)을 사용하여 요청한 data를 얻을수 있습니다  
```python
visit_request_user_id = request.GET['visitRequestUserId']
# key값 not found시 처리 예
visit_request_user_id = request.GET('visitRequestUserId', '')
```

post method의 경우 request.data(dictionary data type)을 사용하여 요청한 data를 얻을수 있습니다  
request.data는 request body에 있는 data를 dictionary 형태로 변환한 data입니다  

```python
visit_request_user_id = request.data['visitRequestUserId']
# key값 not found시 처리 예
visit_request_user_id = request.data.get('visitRequestUserId', '')

```

url의 data를 key 값으로 매핑하여 data로 사용할 수 있습니다
```python
#urls.py
path('api/v1/visits/<int:pk>', visit_views.VisitDetail.as_view()),	#get, delete

#views.py
def delete(self, request, pk):
```

처리한 dictionary형태의 data를 Response 객체에 넣어 client로 return합니다  
resopnse 객체는 dictionary data를 json 객체로 변환하여 client로 return합니다  
```python
resp_dict = {'content': visit_list, 'totalPages' : tot_page, 'totalElements': tot_cnt}
return Response(resp_dict)
```

dictionary to model object setting 예제
```python
v = Visit();
v.visit_request_user_id = req_data['visitRequestUserId']
v.visit_request_user_full_name = req_data['visitRequestUserFullName']
. . .
v.save() 
```

query object to dictionary setting 예제
```python
qs = Visit.objects.filter(delete_yn='N')
. . .
visit_list = []
for row in qs:
	visit_dic    = {
    	'visitId': row.visit_id,
        'visitRequestUserId' : row.visit_request_user_id,
        'visitRequestUserFullName': row.visit_request_user_full_name,
        . . .
    }
    visit_list.append(visit_dic)
```

dictionary와 object간의 상호 변환은 Serializer를 사용하여 간편하게 작업할 수 있습니다  

### function based view

다음은 rest api를 function based view로 작성한 예입니다  
client에서 url=visit/list, method=get으로 호출하면  
views.py 파일의 visit_list_func_exam을 실행하고 request.method == 'GET' 인  
if문으로 분기합니다  
```python
#urls.py
path('visit/list', views.visit_list_func_exam),

#views.py
@api_view(['GET','POST'])
def visit_list_func_exam(request):
    if request.method == 'GET':
        print('# request : ', request.GET)
        visit_id = request.GET.get('visit_id', '')
        qs = Visit.objects.filter(visit_id=visit_id)
        vs = VisitSerializer(qs, many=True)
        return Response(vs.data)
    elif request.method == 'POST':
        return Response({})
        pass
```

### generic view

다양한 generic view를 활용하여 편리하게 작업할 수 있습니다  
```python
#class VisitListViewExam(generics.ListCreateAPIView):
class VisitListViewExam(mixins.ListModelMixin,
                  mixins.CreateModelMixin,
                  generics.GenericAPIView):
    #queryset = Visit.objects.first()
    serializer_class = VisitSerializer

    def get_queryset(self):
        queryset = Visit.objects.all()
        visit_id = self.request.query_params.get('visit_id', None)
        if visit_id is not None:
            queryset = queryset.filter(visit_id=visit_id)
            
        return queryset

    def list(self, request):
        qs = self.get_queryset()
        serializer = self.serializer_class(qs, many=True)
        return Response(serializer.data)        
        
    def create(self, request):
        serializer_data = request.data

        serializer = self.serializer_class(data=serializer_data)
        serializer.is_valid(raise_exception=True)
        serializer.save()

        return Response(serializer.data)
```

django rest framework관련 상세한 내용은 다음 url을 참조합니다
[https://www.django-rest-framework.org/]

## serializer
serializer는 model 객체나 queryset 객체를 dictionary type으로 변환(serialize)하거나  
dictionary type을 model 객체로 변환(deserialize)합니다  

다음은 ModelSerializer를 상속하여 작성한 serializer 예입니다  
```python
class VisitSerializer(serializers.ModelSerializer):
    class Meta:
        model = Visit
        fields = "__all__"
```

다음은 dictionary data를 model 객체로 변환하는 예입니다  
```python
v_dict = req.GET
. . .
v_ser = VisitSerializer(data=v_dict)
valid = v_ser.is_valid()
v_ser.save()
```

다음은 queryset 객체를 dictionary type으로 변환하는 예입니다  
```python
qs = Visit.objects.filter(delete_yn='N')
. . .
visit_list = []
for row in qs:
	row_dict = VisitSerializer(row).data 	#serializer : model object => dict
    . . .
    visit_list.append(row_dict)
resp_dict = {'content': visit_list, 'totalPages' : tot_page, ...}

```