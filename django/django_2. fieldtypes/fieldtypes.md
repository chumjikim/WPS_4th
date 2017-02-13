#Field types

index

- Field types
	- AutoField
	- BigAutoField
	- BigIntegerField
	- BinaryField
	- BooleanField
	- CharField
	- CommaSeparatedIntegerField
	- DateField
	- DateTimeField
	- DecimalField
	- DurationField
	- EmailField
	- FileField
		- FileFiled and Filedfile
	- FilePathField
	- FloatField
	- ImageField
	- IntegerField
	- GenericIPAddressField
	- NullBooleanField
	- PositiveIntegerField
	- PositiveSmallIntegerField
	- SlugField
	- SmallIntegerField
	- TextField
	- TimeField
	- URLField
	- UUIDField

>[Model field reference 문서](https://docs.djangoproject.com/en/1.10/ref/models/fields/)



##AutoField
`class AutoField(options)`  
사용가능한 ID에 따라 자동으로 증가하는 IntegerField이다. 보통은 직접적으로 사용하지 않는다. 별다른 지정을 하지 않으면 primary key field가 모델에 자동으로 추가된다. 

```python
# Automatic primary key fields
# 기본적으로 django각 모델에 다음과 같은 field를 구성한다.
id = models.AutoField(primary_key=True)
```

##BigAutoField
##BigIntegerField
##BinaryField
##BooleanField
##CharField
`class CharField(max_length=None, **options)`

작은것 부터 큰 사이즈의 문자열을 나타내는 문자열 field이다. 이 field의 기본 위젯 형식은 TextInput이다.  
많은 양의 텍스트라면 TextField를 사용해라.

CharField는 추가적인 인자를 받는다.   

__CharField.max_length__  
field의 문자의 최대 길이다. max_length는 데이터베이스 레벨과 django의 validation에 강제된다. 


##CommaSeparatedIntegerField
`class CommaSeperatedIntegerField(max_length=None, **options)`

##DateField
`class Datefield(auto\_now\_add=False, **options)`

###Datefield.auto\_now
object가 __저장될 때 마다__, field를 자동으로 설정한다.  
`'last-modified'`같은 timestamps에 유용하다.  
항상 현재 날짜를 사용한다.  
재정의할 수 있는 값이 아니다. 

###DateField.auto\_now\_add
object가 __처음 생성될 때__, field를 자동으로 설정한다.  
timestamp 생성에 유용하다.  
항상 현재의 날짜를 사용한다.  
재정의할 수 있는 값이 아니다.  
그렇기 때문에 object를 생성할 때 이 field값을 설정하려고 해도 무시된다. 이 field를 수정하고 싶다면 `auto_now_add=True`대신 다음의 설정을 따라야한다. 

- DateField: default=date.today - `datetime.date.today()`
- DateTimeField: default=timezone.now - `django.utils.timezone.now()` 

> __Note__  
> 
>auto_now 혹은 auto\_now\_add를 True로 설정하면 해당 field는 **editable=False**, **blank=True**로 설정된다.
> 
>auto\_now 혹은 auto\_now\_add 옵션은 생성 또는 업데이트 할 때의 기본시간대 날짜를 사용한다. 다른 값을 원한다면 auto_now 혹은 auto\_now\_add를 사용하는 것 대신, 자신만의 호출 가능한 기본값을 사용하거나 save()를 무시할 수 있다. 또는 DateField 대신 DateTimefield를 사용하여 표시 시간에 datetime에서 date로 변환해 처리할 수 있다. 

##DateTimeField
`class DateTimeField(auto\_now=False, auto\_now\_add=False, **options)`

python에서 datetime.datetime 인스턴스로 표현되는 날짜와 시간이다. DateField와 동일한 arguments를 갖는다. 

위젯에서 이 field의 기본 폼은 단일 TextInput이다. admin은 JavaScript shortcuts으로 두개의 분리된 TextInput을 사용한다. 

##DecimalField
##DurationField
##EmailField
##FileField
##FilePathField
##FloatField
##ImageField
##IntegerField
##GenericIPAddressField
##NullBooleanField
##PositiveIntegerField
##PositiveSamllIntegerField
##SlugField
##SmallIntegerField
##TextField
`class TextField(**options)`

큰 text field이다.
이 field의 기본 위젯 형식은 Textarea이다.

만약 max_legth속성을 지정하면, 자동으로 생성된 폼인 Textarea 위젯에 반영된다. 그러나 데이터베이스  레벨이나 모델에는 강제되지않는다. 이를 원한다면 CharField를 사용해라. 


##TimeField
##URLField
##UUIDField

