#[Django] 모델 - 1. Model Syntax


Django에서 모델은 아래와 같은 특징을 가지고 있습니다.

* 각각의 모델은 파이썬 클래스로 표현되며, django.db.models.Model 클래스의 서브클래스입니다.
* 모델 클래스의 어트리뷰트로 데이터베이스의 필드(컬럼)을 표현합니다.
* Django는 이러한 모델 클래스를 통해 데이터베이스 접근 API를 제공합니다.



## Quick example
이 예제는 Person 모델을 정의합니다. 이 모델은 first_name 과 last_name 필드를 가집니다.


```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
```


"first_name"과 "last_name"은 모델의 필드입니다. 각각의 필드는 클래스 어트리뷰트로 명시하며, 각각의 어트리뷰트는 데이터베이스의 컬럼과 매치됩니다.

위의 모델 클래스는 실제 데이터베이스에 아래와 같이 테이블을 만들어줍니다.

```
CREATE TABLE myapp_person (
    "id" serial NOT NULL PRIMARY KEY,
    "first_name" varchar(30) NOT NULL,
    "last_name" varchar(30) NOT NULL
);
```

클래스와 실제 데이터베이스 테이블간의 관계에 대해 좀 더 설명하면 아래와 같습니다.

* 테이블의 이름("myapp_person")은 app이름과 모델 클래스이름의 조합으로 자동생성됩니다. 하지만 직접 정의해줄 수도 있습니다.
* 모델클래스에 명시하지 않았지만 "id" 필드가 자동적으로 추가되었습니다. 자동 생성하지 않도록 할 수도 있습니다.
* 위의 "CREATE TABLE" SQL문은 PostgreSQL 문법을 사용했습니다. 참고로, Django는 대부분의 ORM이 그렇듯이 설정에 따라 코드 변경없이 PostgreSQL 뿐만이 아니라 다양한 데이터베이스 엔진을 사용할 수 있습니다.

##Using models 
모델은 정의한 후에는 INSTALLED_APPS 설정에 모델이 정의된 app을 추가해 주어야 합니다.

예를들어, 모델 클래스가 myapp.models 모듈에 정의되어있다면, INSTALLED_APPS 설정은 아래와 같이 되어야 합니다.

```
INSTALLED_APPS = (
    #...
    'myapp',
    #...
)
```

INSTALLED_APPS 설정에 새로운 app을 추가한 경우에, "manage.py migrate" 스크립트를 실행해 주어야 합니다. 마이그레이션에 대해서는 별도의 문서에서 설명합니다.


## Fields 

모델을 정의할 때 가장 중요한 부분은 바로 필드 선언입니다. 필드는 클래스 어트리뷰트로 표기됩니다. 이때, 모델 클래스에서 기본으로 제공하는 API 메서드들의 이름(clean, save, delete 등)과 겹치지 않도록 주의하세요.

아래 코드는 모델 클래스 선언의 예입니다.

```
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()

```

##Field types

모델에 필드를 선언할때, 각각의 필드는 Field 클래스의 인스턴스여야합니다. 각 필드의 클래스 타입을 통해 Django는 아래와 같은 내용을 자동적으로 판단하여 동작할 수 있게 됩니다.
데이터베이스 컬럼 타입. (예) INTEGER, VARCHAR
Django Form을 이용하여 모델을 HTML 위젯으로 렌더링할때 각 필드를 어떻게 표현할지.

(예)
``` <input type="text">, <select>
```

Django admin 페이지나 자동생성된 form에서 수행될 최소한의 유효성 체크(Validation)
Django는 여러가지 내장된 필드 타입을 제공하며, 직접 만들어 사용할 수도 있습니다.

## Field options

각각의 필드에 옵션을 줄수 있는데, 특정 필드에만 사용할 수 있는 옵션이 있습니다. 예를들어, CharField(그리고 CharField의 서브클래스들)은 필수적으로 max_length 옵션이 주어져야 합니다. 그래야만 실제 DB에 VARCHAR 컬럼을 만들때 길이를 초기화 할 수 있기 때문입니다.

그런가하면 모든 필드타입에 공통적으로 사용할 수 있는 옵션들이 있습니다. 앞의 예와는 달리 이 옵션들은 모두 선택적입니다. 레퍼런스에 각각에 대해 자세히 설명되어있긴 하지만, 자주사용되는 옵션들만 간단히 살펴보도록 하겠습니다.

###null
값이 True 이면, DB상의 해당 컬럼에 NULL 값을 할당할 수 있습니다. 즉, 데이터베이스의 NOT NULL 제약과 관련있는 옵션입니다.

###blank
값이 True이면, 필드값을 입력하지 않아도 됩니다. 기본값은 False 입니다.


null 옵션과는 다릅니다. null은 DB 상의 NOT NULL 제약과 연관된 옵션이라고 했을때, blank 옵션은 DB의 어떤 제약과도 관련이 없습니다. 즉 blank 옵션을 설정한다고 해서 DB상에 어떤 제약이 걸리지는 않습니다. 다만, 유효성 검증(validation)과 연관이 있습니다. Django는 복잡한 구현없이 모델에 대한 HTML 폼을 쉽게 생성할 수 있도록 도와주는데, 이 폼 검증시에 blank 옵션이 False인 필드는 필수 입력 필드로 처리됩니다.


###choices
enum과 같이 필드에 저장할 수 있는 값이 제한적인 경우에 사용할 수 있습니다. 옵션값은 아래와 같이 2중 튜플(또는 리스트)로 설정해야합니다. 

```
YEAR_IN_SCHOOL_CHOICES = (
    ('FR', 'Freshman'),
    ('SO', 'Sophomore'),
    ('JR', 'Junior'),
    ('SR', 'Senior'),
    ('GR', 'Graduate'),
)
```

이때, 내부의 튜플은 2개의 요소를 가지는데 하나는 실제 DB에 저장될 값이고, 두번째는 admin페이지나 폼에서 표시할 이름입니다. choices 옵션이 설정된 필드의 값은 튜플의 첫번째 요소(ex. 'FR') 값이며, 모델 객체에 대해 get_필드명_display() 메서드를 호출하면 튜플의 두번째 요소(ex. 'Freshman') 값을 얻을 수 있습니다. 참고로, get_xxx_display() 메서드는 Django에 의해 자동으로 생성됩니다.

```
from django.db import models

class Person(models.Model):
    SHIRT_SIZES = (
        ('S', 'Small'),
        ('M', 'Medium'),
        ('L', 'Large'),
    )
    name = models.CharField(max_length=60)
    shirt_size = models.CharField(max_length=1, choices=SHIRT_SIZES)
```

![1](/Users/changyukim/Desktop/image/1.png)

위와 같이 선언된 모델을 쉘에서 실습해보면 아래와 같은 결과를 볼 수 있습니다. 

```
>>> p = Person(name="Fred Flintstone", shirt_size="L")
>>> p.save()
>>> p.shirt_size
u'L'
>>> p.get_shirt_size_display()
u'Large'
```

###default
해당 필드의 기본값을 정의합니다. 옵션값은 단순히 값도 될 수 있고 함수도 될 수 있습니다. 만약 함수를 옵션값으로 설정하는 경우 새로운 모델 객체를 만들때마다 해당 함수가 호출됩니다.

###help_text
해당 필드가 폼 위젯으로 표시될때 함께 표시될 도움말입니다. 해당 모델을 폼에서 쓰지 않더라도 모델을 문서화하는데 도움이 됩니다.

###primary_key
값이 True 이면, 해당 필드를 모델의 primary key 로 정의합니다.

기본적으로 Django는 별도로 지정하지 않는 경우, "id"라는 IntegerField 하나를 모델에 자동으로 추가하고, 그 필드에 "primary_key=True" 옵션을 설정합니다. 자동으로 생성되지 않도록 하고 싶다면, 원하는 필드에 primary_key=True 옵션을 주면 됩니다.

이때 주의할 점이 있습니다. 기존의 모델 객체에 primary key 필드값을 변경하고 저장하는 경우, 기존 모델 객체의 primary key 필드값이 바뀌는 것이 아니라, 새로운 모델 객체가 생성됩니다. 물론 기존의 모델객체는 DB상에서 지워지지 않습니다.


```
from django.db import models

class Fruit(models.Model):
    name = models.CharField(max_length=100, primary_key=True)
```


아래의 예제에서 fruit 객체의 pk 값이 바뀌었기 때문에, Django는 이전의 pk값을 알수 없습니다. 그러므로 Django는 PK값을 기준으로 이미 존재하는 레코드이면 update를, PK값이 존재하지 않는경우 create를 수행합니다. 

```
>>> fruit = Fruit.objects.create(name='Apple')
>>> fruit.name = 'Pear'
>>> fruit.save()
>>> Fruit.objects.values_list('name', flat=True)
['Apple', 'Pear']
```

###unique
True 인 경우 해당 필드값은 해당 테이블을 통틀어 유일한 값을 가져야 합니다. 즉, unique 제약이 걸립니다.


##Automatic primary key fields
기본적으로 Django는 모델에 아래와 같은 필드를 자동으로 생성합니다.
```
id = models.AutoField(primary_key=True)
```

위의 필드는 자동증가(auto increment)하는 primary key 필드입니다.

앞에서 언급했듯이, 직접 primary key 필드를 선언하고 싶다면, 모델의 필드 중 하나에 "primary_key=True" 옵션을 주면 됩니다. Django는 모델에 primary_key=True 옵션이 명시적으로 선언된 필드가 있다면 자동적으로 id 필드를 생성하지 않습니다.

참고로 Django의 모델은 필수적으로 하나의 Primary Key 필드를 가져야 합니다. 다시 말해서, 직접 선언했던 자동으로 생성되었던지 관계없이 1개의 primary key 필드를 가져야 합니다.


##Verbose field names
verbose의 사전적 의미는 "수다스러운, 장황한" 등의 의미를 가집니다. 모델에서 verbose field name 이란 앞에서 잠깐 언급한 choices 옵션에서의 display_name 과 비슷합니다. 즉, 읽기좋은 형태의 필드이름입니다.

ForeignKey, ManyToManyField, OneToOneField 는 첫번째 인자로 모델 클래스를 받기 때문에, 해당 타입의 필드에 verbose name을 지정하려면 아래와 같이 키워드 인자로 전달하면 됩니다.

```
poll = models.ForeignKey(Poll, verbose_name="the related poll")
sites = models.ManyToManyField(Site, verbose_name="list of sites")
place = models.OneToOneField(Place, verbose_name="related place")
```



## Relationships
누가뭐래도 RDBMS 의 강점은 바로 "관계"입니다. 테이블간에 관계를 정의하고, 그 관계를 기반으로 쿼리나 DB의 무결성을 보장할 수 있게 해줍니다.

Django는 3가지 대표적인 데이터베이스 관계 형태(일대다, 다대다, 일대일)를 제공합니다. 


###Many-to-one relationships

일대다 관계를 정의하려면 django.db.models.ForeignKey 클래스를 이용하여 필드를 선언하면 됩니다.

ForeignKey 필드 선언시에 관계를 맺을 모델 클래스를 인자로 넘겨주어야 합니다.

Car 모델과 Manufacturer 모델이 일대다 관계라고 가정해 봤을때 (Manufacturer 는 여러개의 Car와 관계를 가지지만, Car는 단하나의 Manufacturer와 관계를 가집니다), 모델 선언은 아래와 같습니다.


```
from django.db import models

class Manufacturer(models.Model):
    # ...
    pass

class Car(models.Model):
    manufacturer = models.ForeignKey(Manufacturer)
    # ...

```

이때, 재귀적인 관계도 가능합니다. 즉, 아래와 같이 ForeignKey 필드를 선언할 수도 있습니다. 단, 첫번째 인자를 클래스가 아닌 클래스 명을 문자열로 전달해야 합니다. 필드가 선언되는 시점에는 아직 클래스가 생성되지 않았기 때문입니다. 즉, 클래스 대신 클래스 명을 인자로 사용함으로써 클래스 선언 순서에 관계없이 참조가 가능합니다.


```
class Employee(models.Model):
    boss = models.ForeignKey('Employee')
    # ...

```

###Many-to-many relationships
다대다 관계를 선언할때는 ManyToManyField를 사용합니다. ForeignKey 필드와 마찬가지로 관계를 가지는 모델 클래스를 첫번째 인자로 받습니다.

예를들어 봅시다. Pizza에는 여러가지의 Topping이 올라가며, Topping은 여러개의 Pizza에 사용될 수 있습니다. 이 경우 모델 선언은 아래와 같습니다.

```
from django.db import models

class Topping(models.Model):
    # ...
    pass

class Pizza(models.Model):
    # ...
    toppings = models.ManyToManyField(Topping)

```

ForeignKey 필드와 마찬가지로 재귀적인 관계를 선언할 수 있으며, 인자로 클래스 대신 클래스 이름을 전달할 수도 있습니다.

ManyToManyField의 필드이름은 복수(plural) 명사로 지어주는 것을 추천합니다.(예를들면, topping이 아니라 toppings).

ManyToManyField는 관계를 가지는 두 모델 클래스 중에 한쪽에만 선언하면 됩니다. 
그러면 어느쪽에 선언해야 할까요? 어느쪽에 선언하더라도 관계는 없습니다만, admin 페이지에서 모델을 편집한다고 했을때 편한 쪽으로 선택하는것이 일반적입니다.

예를들어, 신제품 피자를 시스템에 등록해야 한다고 생각해보세요. 이 경우에 피자 객체를 만들고 거기에 토핑(들)을 추가하는 것이 훨씬 쉽고 자연스럽습니다. 이 경우 Pizza에 toppings 필드를 선언하면 됩니다.



### Extra fields on many-to-many relationships
Pizza와 Topping의 경우에는 ManyToManyField만 선언하면 끝입니다. 하지만 두 모델간의 관계가 추가적인 데이터를 가지는 경우가 있습니다. 

학교와 동아리를 예로 들어 봅시다. 학생은 여러 동아리에 가입할 수 있고, 각 동아리는 여러명의 학생들을 회원으로 갖습니다. 그런데, 동아리에 가입한 날짜를 저장해야합니다. 이 경우 어떻게 해야 할까요?

이 경우 다대다 관계를 가지는 두 개의 모델 이외에 두 모델의 관계와 추가 데이터를 저장할 또다른 모델이 필요합니다. 이를 중간(intermediate) 모델이라고 합니다. 

Pizza와 Topping의 경우에는 Django가 알아서 중간 모델을 만들어 줍니다. 아마도 그 모델은 pizza 와 topping 두개의 필드를 가지는 모델이었을 겁니다. 만약, 이 중간 모델을 직접 선언하려면 아래와 같이 할 수 있습니다.

```
from django.db import models

class Person(models.Model):
    name = models.CharField(max_length=128)

    def __str__(self):              # __unicode__ on Python 2
        return self.name

class Group(models.Model):
    name = models.CharField(max_length=128)
    members = models.ManyToManyField(Person, through='Membership')

    def __str__(self):              # __unicode__ on Python 2
        return self.name

class Membership(models.Model):
    person = models.ForeignKey(Person)
    group = models.ForeignKey(Group)
    date_joined = models.DateField()
    invite_reason = models.CharField(max_length=64)
```

중간 모델을 직접 선언할 때는, 관계를 가지는 두 모델에 대한 ForeignKey 필드를 선언하고 추가적인 필드를 선언하면 됩니다.

* 중간 모델을 직접 선언하는 경우에 아래와 같이 몇가지 제약사항이 있습니다.
중간모델은 원본모델(위의 예에서 Group 모델)에 대해 단 하나의 ForeignKey 필드를 가져야 합니다. 여러개도 안되고 없어도 안됩니다. 아니면, ManyToManyField 에 through_fields 옵션으로 관계에 사용되는 필드 이름을 지정해주어야 합니다. 둘중 하나가 아니면 Validation 에러가 발생합니다. 타겟 모델(위 예제에서 Person)의 경우에도 동일합니다.

* 자기자신에게 다대다 관계를 가지는 모델의 경우에는 중간 모델에 동일한 모델에 대한 ForeignKey 필드를 2개 선언할 수 있습니다. 3개 이상의 ForeignKey 필드를 선언하는 경우에는 앞에서 언급한것과 같이 though_fields 옵션을 설정해주어야 합니다.

* 자기자신에게 다대다 관계를 가지고 중간모델을 직접 선언하는 경우에 ManyToMany 필드의 symmetrical 옵션을 False 로 설정해야 합니다. 



###One-to-one relationships

일대일 관계를 정의하려면, OneToOneField를 이용하면 됩니다. 다른 관계 필드와 마찬가지로 모델 클래스의 어트리뷰트로 선언하면 됩니다. 

일대일 관계는 다른 모델을 확장하여 새로운 모델을 만드는 경우 유용하게 사용될 수 있습니다.

OneToOneField는 관계를 맺는 모델 클래스를 첫번째 인자로 받습니다.

예를들어, 당신이 "가게(Places)" 정보가 담긴 데이터베이스를 구축하고 있다고 합시다. 아마도 데이터베이스에 주소, 전화번호 등의 정보가 들어가게 되겠지요. 그런데, 맛집 데이터베이스를 추가적으로 구축하게 되었습니다. 이럴때, 새로 Restaurant 모델을 만들어, 주소나 전화번호 등의 필드를 다시 선언할 수도 있겠지만, 이러한 반복을 피하기 위해 Restaurant 모델에 Place 모델에 대한 OneToOneField를 선언할 수도 있습니다. 

ForeignKey 필드와 마찬가지로 자기자신 또는 아직 선언되지 않은 모델에 대해서도 관계를 가질 수 있습니다.

OneToOneField 필드는 parent_link 라는 옵션을 제공합니다. 이 옵션에 대한 설명은 "model field reference"를 참고하세요.

OneToOneField 클래스가 자동적으로 모델의 Primary Key가 되었던 적이 있었더랬습니다. 하지만 지금은 더이상 그렇지 않습니다. 물론 직접 primary_key=True 로 지정하여 Primary Key로 만들 수는 있습니다. 어쨋든 결과적으로 이러한 이유 때문에 하나의 모델이 여러개의 OneToOneField를 가질 수 있게 되었습니다.


###Models across files
다른 앱에 선언된 모델과 관계를 가질 수 있습니다. 그렇게 하려면, 다른 앱의 모델을 import 해서 아래와 같이 관계 필드를 선언하면 됩니다.

```
from django.db import models
from geography.models import ZipCode

class Restaurant(models.Model):
    # ...
    zip_code = models.ForeignKey(ZipCode)

```


### Meta options
아래와같이 모델클래스 내부에 Meta 라는 이름의 클래스 선언해서 모델에 메타데이터를 추가할 수 있습니다.

```
from django.db import models

class Ox(models.Model):
    horn_length = models.IntegerField()

    class Meta:
        ordering = ["horn_length"]
        verbose_name_plural = "oxen"
```
"모델 메타데이터"란 앞서 보았던 필드 단위의 옵션들과 달리 모델 단위의 옵션이라고 볼 수 있습니다. 예를들면, 정렬 옵션(ordering), 데이터베이스 테이블 이름(db_table), 또는 읽기 좋은 이름이나 복수(plural) 이름을 지정해 줄 수 있습니다(verbose_name, verbose_name_plural). 모델클래스에 Meta 클래스를 반드시 선언해야 하는 것은 아니며, 모든 옵션을 모두 설정해야 하는 것도 아닙니다.


## django 모델 상속 ( Model Inheritance )

django의 ORM ( Object-Relational Mapping ) 기능은 데이터베이스 구조를 머리속 그림대로 직관적으로 구현하는 데에 매우 편리하다. 만약 당신이 SQL에 숙련되지 못했다면 엄두를 내지 못할 DB 구조를 django에서는 파이썬 코드를 통해 비교적 쉽게 해결할 수 있는 것이다. 이번에는 수 많은 django의 ORM 기능 중에서 상속 기능만 간단히, 그리고 부분적으로 설명해 보고자 한다.

django는 세 가지의 모델 상속 타입을 제공하는데 abstract base classes, multi-table inheritance, proxy model 이라는 이름으로 django doc에서 설명한다. 객체 지향적인 프로그래밍을 요즘은 모두들 아시기 때문에 상속에 대해서 설명하진 않을 것이다. 기본적으로 상속이라는 단어의 사전적 의미에서 알 수 있듯이 뭔가 타겟을 정해서 그 녀석을 통째로 받아서 이용해 먹는거라고 생각하면 된다.

###1. Abstract base classes

가장 특별하지 않은 상속 방법이다. 말보다는 코드를 먼저 보겠다. 운송수단에 관한 모델을 구성하려고 한다.


```
class Vehicle( models.Model ):
    name = models.CharField( max_length = 100 )
    birthday = models.DateField( )
    created = models.DateTimeField( auto_now_add = True )

    class Meta:
        abstract = True

class Car( Vehicle ):
    type = models.CharField( max_length = 100 )

class Automobile( Vehicle ):
    pass

class Bus( Vehicle ):
    num_passengers = models.IntegerField( )

```

가장 위에 운송수단 전체를 포함하는 필드들을 가진 Vehicle 클래스 (앞으로 mother class의 의미로 '엄마'라고 부르겠다) 가 선언됐고 이 것을 상속받는 세 개의 모델이 Car, Automobile, Bus (child class의 의미로 '새끼'라고 부르겠다) 이다.

Vehicle은 이름, 제조일을 가진 모델이고, Car는 필드가 타입 하나 뿐이지만 Vehicle한테 물려받은게 있으니까 이름, 제조일, 타입 이렇게 세 개의 필드를 가지고 있을 것이다. 그렇다면 Automobile은? Vehicle의 필드만 고대로 갖고 있겠지. 그렇다면 나는 네 개의 모델 클래스가 있으니까 네 개의 DB 테이블이 생성되는 것일까. 그렇지 않다

메타 옵션에서 abstract = True 를 설정하면 엄마 모델은 실제로 또는 물리적으로 존재하지 않는 가상의 클래스가 된다. 그리고 새끼 모델들은 엄마의 필드와 속성, 함수들을 다 물려받아 실체가 있는 DB 테이블이 된다. 그러니까 나는 세 개의 DB 테이블을 갖게 되는 것이다. 이를 확인하고 싶다면 makemigrations를 통해 생성된 migrations 폴더 안의 파이썬 코드를 열어보면 migrate를 하기 전에 확인할 수 있을 것이다. 물논 직접 migration을 해보면 당연히 확인할 수 있겠지만.

abstract base를 사용한다는 것은 새끼 모델들이 엄마 없이 각각 독립적인 DB 테이블로서 존재하며, 새끼와 엄마의 상속관계는 실제로 없는 것이다. 공통된 필드가 많이 있는 모델 클래스들이 있을 때 코드를 효율적으로 사용하기에 편리한 기능이라고 생각한다.


###2. Multi-table inheritance

위 경우와는 다르게 실제로 상속이 일어나는 타입이다? 코드를 보자.

class Vehicle( models.Model ):
    name = models.CharField( max_length = 100 )
    birthday = models.DateField( )
    created = models.DateTimeField( auto_now_add = True )

class Car( Vehicle ):
    type = models.CharField( max_length = 100 )

class Automobile( Vehicle ):
    pass

class Bus( Vehicle ):
    num_passengers = models.IntegerField( )


똑같은 예제 코드를 썼다, 메타 클래스에 abstract = True 가 사라진 것 빼고. 파이썬 클래스 상속하듯이 그냥  쓰면 되는 것이다. 하지만 이로인해 생성될 DB 구조는 많이 달라진다.

이 경우 나는 네 개의 모델 클래스를 통해 정말로, 정직하게 네 개의 DB 테이블을 갖게 된다. 그렇다면 새끼는 엄마를 상속받으니까... 엄마 테이블은 새끼들의 내용을 다 가지고 있는 포괄적인 목록이 되겠네? 그렇다.

좀 더 자세하게 이야기해보자. 이것은 django shell script를 통해 이해하는 것이 가장 효과적이라고 생각한다.


```
>>> v = Vehicle( )
>>> v.name = 'Accent'    # Vehicle의 필드이므로 아무 문제 없다
>>> v.num_passenger = 50    # 당연히 에러
>>> v.save( )    # 실제로는 나머지 필드인 birthday도 채우고 저장해야 할 것이다 (왜?)

>>> b = Bus( )
>>> b.num_passenger = 50    # OK
>>> b.name = 'School Bus'    # 엄마의 필드까지 모두 가지고 있기 때문에 OK
>>> b.save( )

```

자 여기까지 하고 실제 DB 테이블에 어떻게 저장돼있는지 보자. 우선 bus 테이블은 당연히 한 줄이 있을 것이다. 그리고 예상 가능하게도 vehicle 테이블에는 두 줄이 있다. 당연하게도 vehicle 테이블에는 우리가 잘 선언한 필드 name, birthday, created timestamp를 갖고 있을 것이다. 그리고 bus 테이블에서는 역시 선언한 필드 num_passengers와 연결된 vehicle 녀석의 pk가 저장돼있음을 알 수 있다. 그럼 나는 이 연결된 것을 어떻게 찾아갈 수 있을까.


```>>> v = Vehicle.objects.get( name = 'School Bus' )
>>> v.bus    # v는 Bus 클래스를 갖고 있기 때문에 연결된다
>>> v.bus.num_passenger    # 50임을 확인할 수 있다

>>> v.car    # 에러가 날 것이다는 것은 모두가 알 것이고, Car.DoesNotExist 에러가 난다

```

소문자로 연결할 수 있는데, 여기서 문제는 v가 Bus 클래스를 갖고 있다는 사실을 이미 안 상태에서 내가 직접 v.bus라고 타이핑을 통해 연결한다는 것이다. 가상으로 Vehicle 테이블에 정말 많은 줄을 저장해놨다고 생각해보자. 내가 원하는 가장 이상적인 시나리오는 일일이 누가 어느 클래스인지 모르는 상태에서도 Vehicle 클래스에서 연결돼있는 subclass로 바로 연결되는 것이다. 그렇지 못한다면 나는 거꾸로 새끼 모델에서부터 접근해야하는데 그것은 자연스럽지 못한 순서이다. 엄마가 모든 것을 다 갖고 있는데 왜 있을지 없을지도 모르는 새끼에게 먼저 가야 하는 것인가.

이 문제의 해결은 django 에서 바로 지원되지는 않는 기능이지만 외부 모듈을 통해 해결할 수 있다. django-model-utils의 InheritanceManager 를 이용하면 다음과 같은 코드로 내가 바라는 바를 해결해 준다.

```
from model_utils.managers import InheritanceManager

class Vehicle( models.Model ):
    name = models.CharField( max_length = 100 )
    birthday = models.DateField( )
    created = models.DateTimeField( auto_now_add = True )

    objects = InheritanceManager( )    # objects 매니저를 새걸로 바꿔준다

class Car( Vehicle ):
    type = models.CharField( max_length = 100 )

class Automobile( Vehicle ):
    pass

class Bus( Vehicle ):
    num_passengers = models.IntegerField( )



>>> Vehicle.objects.select_subclasses( )    # 내가 원했던 결과를 확인할 수 있다.

>>> Vehicle.objects.get_subclass( name = 'Accent' )    # 하나만 갖고올 때는 다른 명령어.

```

실제로 DB의 구성은 굉장히 중요하기 때문에 개발을 시작하기 전에 가장 많은 시간과 공을 들이는 문제이다. 상속관계를 잘 이용하면 또는 알고만 있더라고 DB를 구성하는 데 많은 도움이 될 수 있을 거라고 생각한다.


###3. Proxy models
proxy의 사전적 의미는 '대리인'이다. Proxy model은 이미 존재하는 모델을 그대로 받아들이면서 새로운 기능을 추가할 수 있다. proxy model의 대표적인 사용 예제는 User model에 나만의 method를 추가하는 것이다. 나는 django가 기본적으로 제공하는 기능을 최대한 손 대지 않고 사용하고 싶기 때문에, 사용자 모델에 추가적인 필드를 추가하거나 하고 싶을 때는 built-in User model과 연결해서 새로운 model을 다음과 같이 만들어서 사용하곤 한다.



```
from django.contrib.auth.models import User

class UserProfile(models.Model):
    user = models.OneToOneFIeld(User)
    # ... my custom fields ...
    age = models.IntegerField()

    def my_custom_method(self):
        # ... do something ...

# usage

user = User.objects.get(pk=1)
user_age = user.userprofile.age
user_method = user.userprofile.my_custom_method()


```

usage에서 볼 수 있듯이, 1-1으로 relation이 잡혀있는 UserProfile 모델에 접근해서 그 곳에 정의된 field와 method를 사용할 수 있다. 만약 내가 추가적인 필드는 필요하지 않고 custom method만 필요하다면, 이 1-1 모델은 꽤나 잉여스러운 모델이 될 것이다. 이 경우 proxy model을 이용하면 built-in User model을 그대로 가져갈 수 있다. 다음 예제를 보자.

```
from django.contrib.auth.models import User

class UserMethod(User):
    class Meta:
        proxy = True

    def my_custom_method(self):
        # ... do something ...
```

built-in User model을 상속하는 UserMethod model을 만들었고, 거기에 아무런 필드를 추가하지 않고 proxy = True라는 Meta 옵션만 주었다. 이제 이 UserMethod model은 마치 대리인처럼 기존 User model의 내용을 모두 가지고 있는 instance를 뱉어낸다. 그리고 이 대리인은 proxy model에서 정의한 custom method를 사용할 수 있다.

```
proxyuser = UserMethod.objects.get(pk=1)
print proxyuser.username
# built-in User model의 필드 username에 접근할 수 있다.

method_result = proxyuser.my_custom_method() # custom method 실행

```

출처: http://dgkim5360.tistory.com/entry/django-model-inheritance [개발새발로그]




