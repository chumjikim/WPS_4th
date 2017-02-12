
##git 기본 명령어
###1. local repositary 만들기
1. mkdir로 디렉토리 생성
- git 폴더 생성 : git init
- git status로 상태 확인 : git status ~
- stage에 올리기 : git add ~
- commit : git commit ~ (git commit -m "~~" 커밋메세지 작성
  후 바로 commit 하는 방법)
- commit hitory 보는법 : commit log
- 변경된 전체 파일을 한꺼번에 stage로 올리는법 : git add -A
- stage 생략하고 commit하는 방법 : git commit -a
- stage 생략하고 commit 메세지까지 생략하는 방법 : git commit -a -m "한꺼번에 커밋하기"
- git clone : remote 저장소를 통째로 워킹 디렉토리에 복사 (git log를 보면 commit 했던 기록도 볼수 있다....)
- commit 이전에 변경된 사항 보는법 : git log -p
- git push 로 마무리

###2. reset방법
1. 특정 commit으로 돌아가겠다 : git reset --hard commit번호
2. 취소를 취소 : git reset --hard ORIG_HEAD
3. 특정 commit만 골라서 취소 : git revert commit번호

###3. remote repositary 만들기

###4. git ignore

1. 필요없는 conflict를 방지
2. 사용법 : vi .gitingnore 생성 후 ignore할 파일 채워넣기

###5. branch 활용
1. git branch (생성할 branch명)
- git branch : branch 체크
- git checkout (이동할branch명)
- git merge (commit번호)
- git log --graph( graph를 통한 log 분석 가능)

####6. 순서도
1. git init
2. .gitignore -> ingore 할 파일 추가
3. git add
4. initial commit 
5. branch 생성
6. 작업 후 merge


##과제
* 버전관리란 무엇이고, 버전관리 방법에는 어떤 것들이 있는지 알아보기
* git 명령어(command) 정리하기
* 위 두 가지는 오늘 날짜의 과제 제출 폴더에 Mark Down 문법으로 정리

