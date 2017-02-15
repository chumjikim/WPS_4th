#인스타그램 이미지넣기 & 코멘트 달기

##1. 이미지 넣기
###setting.py 설정
1. 맨 윗줄에 내용 추가

```python
MEDIA_ROOT = os.path.join(BASE_DIR, 'media')
```

2. TEMPLATES에  추가할 내용

```python
'django.template.context_processors.media',
```

3. 맨밑에 추가할 내용

```python
MEDIA_URL = '/media/'
```

4. url.py설정

urlpatterns에 추가

```python
+ static(
    settings.MEDIA_URL,
    document_root=settings.MEDIA_ROOT
)
```


##2. 코멘트 달기
###Post List를 보여주는 화면을 구성

1. View에 post_list함수 작성
2. Template에 post_list.html파일 작성
3. View에서 post_list.html을 render한 결과를 리턴하도록 함

```python
def post_list(request):
    return render(request, 'post/post_list.html')
```


4. instagram/urls.py에 post/urls.py를 연결시킴 (app_name은 post)

```python
    url(r'^post/', include('post.urls')),
```
5. '/post/'로 접속했을 때 post_list View에 연결되도록 post/urls.py에 내용을 작성

```python
    url(r'^$', views.post_list, name='post_list'),
```
6. 전체 Post를 가져오는 쿼리셋을 context로 넘기도록 post_list 뷰에 구현

```python
def post_list(request):
    """
    각 Post objects에 Comment 목록을 추가

    """
    post = Post.objects.all()
    context = {
        'post_list': post
    }
    return render(request, 'post/post_list.html', context)
```
7. post_list.html에서 {% for %} 태그를 사용해 post_list의 내용을 순회하여 표현

```python
{% block content %}

{% for post in post_list %}

    {{ post.id }}<br>
    {{ post.author.username }}<br>
    <img src="{{ MEDIA_URL }}{{ post.photo }}"><br>
    {{ post.content|linebreaks|truncatechars:150 }}


    {% if post.comment_set.all %}
        {% for comment in post.comment_set.all %}
            <div>
            {{ comment.content }}
            </div>
        {% endfor %}
    {% else %}
        <p>No comments</p>
    {% endif %}
<hr>
{% endfor %}
{% endblock %}
```

### Post_detail작성
- Post_detail은 하나의 Post에 대한 상세화면

1. View에 post_detail함수 작성

```python
def post_detail(request)
	return(request, 'post/post_detail.html')
```

2~4는 위의 설명과같음

5. '/post/<숫자>/'로 접속했을 때 post_detail View 에
    연결되도록 post/urls.py에 내용 작성. 이 때, post_id라는 패턴명을 가지도록 정규표현식 작성

```python
    url(r'^(?P<post_id>[0-9]+)/', views.post_detail, name='post_detail'),
```
6. url인자로 전달받은 post_id에 해당하는 Post객체를 	context에 넘겨 post_detail화면을 구성


```python
def post_detail(request, post_id):
 post = Post.objects.get(id=post_id)
        context = {
            'post': post,
        }
    return render(request, 'post/post_detail.html', context)
```


###post_detail에 댓글작성기능 추가

1. request.method에 다라 로직 분리되도록 if/else블록 추가
2. request.method가 POST일 경우 request.POST에서 'content'키의 값을 가져옴
3. 현재 로그인한 유저는 request.user로 가져오며 Post의 id값은 post_id인자로 전달되므로 두 내용을 사용
4. 위 내용물과 content를 사용해서 Comment객체 생성 및 저장
5. 다시 아까 페이지 (post_detail)로 redirect해줌

```python
d):
    if request.method == "POST":
        # 전달받은 POST데이터에서 'content'값을 할당
        content = request.POST['content']
        # HttpRequest에는 항상 User정보가 전달된다
        user = request.user
        # URL인자로 전다된 post_id값을 사용
        post = Post.objects.get(id=post_id)

        # Comment객체를 만들어준다
        Comment.objects.create(
            author=user,
            post=post,
            content=content
        )
        # 위의 코드를 이렇게도 사용 가능~
        # post.add_comment(user, content)
        return redirect('post:post_detail', post_id=post_id)
    else:
        post = Post.objects.get(id=post_id)
        context = {
            'post': post,
        }
    return render(request, 'post/post_detail.html', context)
```