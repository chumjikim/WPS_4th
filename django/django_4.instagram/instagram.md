# 인스타그램 프로젝트
## 프로젝트 Flow

1. 프로젝트로 사용할 폴더 생성
2. pyenv virtualenv 3.4.3 <환경명>
3. pyenv local <환경명>
4. pip install django
5. django-admin startproject <프로젝트명>
6. mv <프로젝트명> django_app
7. Pycharm interpreter세팅
8. `django_app`폴더를 `Sources root`로 설정

**프로젝트명**  
instagram

## DB모델 설계

### member app

#### MyUser모델

Attributes

- username
	- 유일한 값을 갖도록 함
- last_name
- first_name
- nickname
- email
- date_joined
- last_modified
- following (팔로우하고 있는 User목록)

Methods

- follow (팔로우 처리)
	- 인자로 다른 MyUser객체를 받아 해당 MyUser객체를 팔로우하도록 함
- unfollow (언팔로우 처리)
	- 위와 반대 동작
- followers (자신을 팔로우하고 있는 User목록, Property처리)
- change_nickname

### post app

#### Post 모델

Attributes

- author (ForeignKey, User)
- photo
- like_users (Intermediate모델로 PostLike를 사용)
- content
- created_date

Methods

- add_comment
- like_count (property)
- comment_count (property)


#### Comment 모델

Attributes

- author (ForeignKey, User)
- post (ForeignKey, Post)
- content
- created_date

#### PostLike 모델 (중간자 모델)

Attributes

- user (ForeignKey, User)
- post (ForeignKey, Post)
- created_date


##실제코드

###member/models.py

####class
```python
class MyUser(models.Model):
    username = models.CharField(
        max_length=20,
        unique=True)
    last_name = models.CharField(
        max_length=20)
    first_name = models.CharField(
        max_length=20)
    nickname = models.CharField(
        max_length=20)
    email = models.EmailField(blank=True)
    date_joined = models.DateTimeField(
        auto_now_add=True
    )
    last_modified = models.DateTimeField(
        auto_now=True)
    following = models.ManyToManyField(
        'self',
        related_name='follower_set',
        verbose_name='follow_list',
        symmetrical=False,
        blank=True,
    )
```

* following은 유저간에 일어나는 행위이다.(한 유저가 여러유저를 팔로우 할 수 있고, 나를 여러 유저가 팔로잉할 수 있음...) 그렇기때문에 MTM참조시 자기자신을 self로 참조한다. 
* symemetrical=false, 내가 타인을 팔로우한다고 해서 타인도 자동적으로 나를 팔로우 하는것이 아니기 때문에 false로 설정해준다.

####methods

```
    def __str__(self):
        return self.username

    def follow(self, user):
        self.following.add(user)

    def unfollow(self, user):
        self.following.remove(user)

    @property  # (읽기전용)
    def followers(self):
        return self.follower_set.all()

    def change_nickname(self, new_nickname):
        self.nickname = new_nickname
        self.save()

    @staticmethod
    def create_dummy_user(num):
        created_count = 0
        last_name_list = ['방', '이', '박', '김']
        first_name_list = ['민아', '혜리', '소진', '아영']
        nickname_list = ['빵', '리헬', '쏘지', '율곰']
        for i in range(num):
            try:
                MyUser.objects.create(
                    username='User{}'.format(i + 1),
                    last_name=random.choice(last_name_list),
                    first_name=random.choice(first_name_list),
                    nickname=random.choice(nickname_list),
                )
                created_count += 1
            except IntegrityError as e:
                print(e)
        return created_count

    @staticmethod
    def assign_global_variables():
        # sys모듈은 파이썬 인터프리터 관련 내장모듈
        import sys

        # __main__모듈을 module변수에 할당
        module = sys.modules['__main__']

        # MyUser객체 중 'User'로 시작하는 객체들만 조회하여 users변수에 할당
        users = MyUser.objects.filter(username__startswith='User')

        # user를 순회하며
        for index, user in enumerate(users):
            # __main__모듈에 'u1, u2, u3, ...' 이름으로 각 MyUser객체를 할당
            setattr(module, 'u{}'.format(index + 1), user)

```

create_dummy_user와 assign_global_variables는 실습을 위해 임의로 생성한 메서드이다. 


```python
##10개의 유저를 만든다
MyUser.assign_global_variables(10)
In [14]: u1.following.add(u1)
In [15]: u1.following.add(u2)
In [16]: u1.following.add(u3)
```
10개의 임의의 유저를 생성시키고, u1,u2,u3가 u1을 팔로우하도록 한다.

```python
In [17]: u1.following.all()
Out[17]: <QuerySet [<MyUser: User1>, <MyUser: User2>, <MyUser: User3>]>
```
u1을 팔로우하는것이 u1,u2,u3임을 확인

```python
In [8]: u2.follower_set.all()
Out[8]: <QuerySet [<MyUser: User1>]>
```
u2로 부터 누구를 팔로우하고 있는지 확인이 가능하다.



###post/models.py
```python
class Post(models.Model):
    author = models.ForeignKey(MyUser)
    photo = models.ImageField(
        upload_to='post', blank=True)
    like_users = models.ManyToManyField(
        MyUser,
        through='PostLike',
        related_name='like_post_set',
    )
    content = models.TextField(blank=True)
    created_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return 'Post[{}]'.format(self.id)

    def toggle_like(self, user):
        # 현재 인자로 전달된 user가 해당 Post(self)를 좋아요 한 적이 있는지 검사
        if self.like_users.filter(id=user.id).exits():
            # 만약에 이미 좋아요 했을 경우 해당 내역을 삭제
            PostLike.objects.filter(post=self, user=user).delete()
        # 아직 내역이 없을 경우 생성해준다
        else:
            PostLike.objects.create(post=self, user=user)

            # 이렇게도 쓸수있다~
            # def toggle_like(self, user):
            #     # 현재 인자로 전달된 user가 해당 Post(self)를 좋아요 한 적이 있는지 검사
            #     p1_list = PostLike.objects.filter(post=self, user=user)
            #     p1_list = self.postlike_set.filter(user=user)
            #     if p1_list.exits():
            #         # 만약에 이미 좋아요 했을 경우 해당 내역을 삭제
            #         p1_list.delete()
            #     # 아직 내역이 없을 경우 생성해준다
            #     else:
            #         PostLike.objects.create(post=self, user=user)


            # 파이썬 삼항연산자
            # [True일 경우 실행할 구문] if 조건문 else [False일 경우 실행할 구문]
            # p1_list.delete() if p1_list.exists() else PostLike.objects.create(post=self, user=user)

    def add_comment(self, user, content):
        return self.comment_set.create(
            user=user,
            content=content
        )

    @property
    def like_count(self):
        return self.like_users.count()

    @property
    def comment_count(self):
        return self.comment_set.count()


class Comment(models.Model):
    author = models.ForeignKey(MyUser)
    post = models.ForeignKey(Post)
    content = models.TextField()
    created_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return 'Post{[]}\'s Comment[{}]'.format(
            self.post_id,
            self.id,
            self.author_id,
        )


class PostLike(models.Model):
    user = models.ForeignKey(MyUser)
    post = models.ForeignKey(Post)
    created_date = models.DateTimeField(auto_now_add=True)

    class Meta:
        unique_together = (
            ('user', 'post'),
        )

    def __str__(self):
        return 'Post[{}]\'s Like[{}]'.format(
            self.post_id,
            self.id,
        )
        
```

Post model은 MyUser와 Post, 그리고 중간자 모델인 PostLike 클래스로 형성된다.

post는 등록자와 내용 필드를 가지며 like_users는 해당 게시물에 좋아요를 누른 사람을 저장해준다. 한사람은 여러 게시물에 대해 좋아요를 누를 수 있기 때문에 MTM로 MyUser를 참조한다. 
toggle_like메서드는 만약 유저가 해당 게시물을 좋아요 했다면 좋아요를 취소시키고, 좋아요를 하지 않았다면 좋아요를 하게 한다.

Meta 클래스는 클래스 안쪽에서 해당 클래스의 속성을 나타내주는 클래스로, 여기에서는 한 유저가 같은 포스트에 여러번 좋아요를 누를 수 없도록 unique속성값을 주었다.(unique_together로 선언한 이유는 user와 post 둘다 만족하는 값에 대해서만 unique속성을 지정하기위해)
