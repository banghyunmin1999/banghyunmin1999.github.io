# BaseModel

basemodel 설치 하기


```python
!pip install pydantic
```

pydantic 설치 확인


```python
!pip show pydantic
```

BaseModel import 해보기


```python
from pydantic import BaseModel
```

BaseModel 필드 정의 하기, 데이터 타입 지정


```python
class TestBaseModel(BaseModel):
    name : str
    age : int
    person : bool = True # 기본값 설정을 미리 설정 가능
```

인스턴스 생성


```python
item_1 = TestBaseModel(
    name = "bang",
    age = 27 
)
```


```python
print(item_1.name)
print(item_1.age)
print(item_1.person)
print(item_1) # BaseModel 객체 전체 출력
```

    bang
    27
    True
    name='bang' age=27 person=True
    

BaseBodel 유효성 검사 (@field_validator)


```python
from pydantic import field_validator

class TestFieldValidator(BaseModel):
    name : str
    age : int
    person : bool = True

    @field_validator('age')
    def f_validator_1(cls, v):
        if v >= 0:
            print(f"{v}는 정상적인 나이입니다")
        else :
            raise ValueError("비정상적인 나이입니다")
        return v

```

테스트 해보기


```python
try: 
    item_2 = TestFieldValidator(
        name = "bang_2",
        age = -1
    )
except Exception as e: 
    print(f"오류가 발생했습니다. 내용 :{e}")
```

    오류가 발생했습니다. 내용 :1 validation error for TestFieldValidator
    age
      Value error, 비정상적인 나이입니다 [type=value_error, input_value=-1, input_type=int]
        For further information visit https://errors.pydantic.dev/2.12/v/value_error
    

중접모델(메인 모델 안에 서브 모델)


```python
class Dimensions(BaseModel):
    width: int
    height: int

class Product(BaseModel):
    id: str
    dimensions: Dimensions

product = Product(
    id = "a_1",
    dimensions = Dimensions(
        width = 1,
        height = 2
    )
)

product_dict = Product(
    id = "a_1",
    dimensions = { # (딕셔너리) 사용도 가능
        "width": 1,
        "height": 2
    }
)

```

중접 모델 접근


```python
print(product.dimensions.width)
print(product_dict.dimensions.width)
```

    1
    1
    

생선된 인스턴스 딕셔너리로 변환(직렬화) 사용목적 : 파이썬 내부에서 데이터 처리 및 조작


```python
convert_dict = product.model_dump()
print(convert_dict)
print(type(convert_dict))
```

    {'id': 'a_1', 'dimensions': {'width': 1, 'height': 2}}
    <class 'dict'>
    

json 문자열로 변환 (직렬화) 사용목적 : 외부 전송 (HTTP 응답, 파일 저장)


```python
convert_json = product.model_dump_json()
print(convert_json)
print(type(convert_json))
```

    {"id":"a_1","dimensions":{"width":1,"height":2}}
    <class 'str'>
    

딕서너리를 다시 모델 인스턴스로 생성(역 직렬화)


```python
new_product = Product.model_validate(convert_dict)
print(new_product)
```

    id='a_1' dimensions=Dimensions(width=1, height=2)
    
