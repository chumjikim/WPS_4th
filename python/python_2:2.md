* pip freeze > requirements.txt
repository를 받았을때 버젼을 맞춰주기 위한 명령어 (> 덮어쓰기)

pip install requsts
pip install beautifulsoup4

<다른사람의 프로젝트를 불러와 가상환경 시스템 및 필요한 setting값을 설치하는 방법>

project/python에 가상환경을 적용한다

```
1. pyenv virtualenv <versions> <env_name>을 환경 생성
2. 해당 폴더에서 pyenv local <env_name>으로 가상환경 사용 설정
3. pyenv versions로 설정 적용되었는지 확인, pip list로 새 가상환경 상태인지 확인
4. pip install -r requirments.txt로 패키지 설치
```
<예시>


* pyenv virtualenv 3.4.3 cloned_project 

* pyenv local cloned_project 

* pip install -r requirements.txt 

* pip install -U setuptools

<제거할때>

* pyenv uninstall


<학습내용>

* 메서드 : 클래스 함수(함수가 클래스 내에 포함된 형태)
* 인스턴스 : 클래스에 의해 만들어진 객체. 예) kim = programmer() 이렇게 만들어진 kim은 객체, kim이라는 객체는 programmer의 인스턴스이다.    
즉, 인스턴스라는 말은 특정 개체가 어떤 클래스의 객체인지 관계 위주로 설명할 때 사용된다. 

