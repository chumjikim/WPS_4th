## 1.BeatuifulSoup이란?
웹페이지를 표현하는 html은 마크업 언어로 태그, 요소, 속성 등의 구성요소를 이용해 문서 구성을 구조적으로 표현한다. 

구조화된 문서는 효율적으로 파싱(탐색)하고 원하는 정보를 찾아낼 수 있다.

beautifulSoup는 HTML과 XML 파일에서 데이터를 읽어내는 파이썬 라이브러리로

파서 트리를 탐색, 검색, 수정하는데 간편하고 사용자가 만든 파서와 함께 사용하기 쉽다.

* bs4는 beautifulSoup4를 뜻한다.


## 2. 웹툰 목록을 가져오는 법

### 1. BeautifulSoup을 이용하여 원하는 페이지에서 주소를 이용해 정보값을 불러올 환경을 setting한다.
```
import requests
from bs4 import BeautifulSoup
```
  * bs4는 beautifulSoup4를 뜻한다.


###2. 실제 주소를 이용하여 웹툰목록의 정보를 파싱해보기
http://comic.naver.com/webtoon/list.nhn?titleId=119874&weekday=tue&page=1

해당 링크는 일정한 패턴을 가진다

* titleId=119874
* page=1

이 값을 이용하여 원하는 페이지의 정보값을 파싱해올 수 있다.

웹툰리스트의 각 제목에서 

```
<td class="title">
```
위 태그가 반복적으로 나타난다. 

또한 제목을 구성하는 전체적인 html은 다음과 같다. 

```
<td class="title">
	<a href="/webtoon/detail.nhn?titleId=20853&no=538&weekday=fri"onclick="clickcr(this,'lst.title','20853','538',event)">534. 에티켓 시즌 1 </a>
</td>
```

첫째로 td태그로 우리가 원하는 정보가 감싸져 있으며
웹툰의 제목은 a 태그의 text,링크는 a 태그의 href속성으로 나타나있다.

이를 이용한 적절한 연산으로 원하는 정보값을 파싱할 수 있다.

