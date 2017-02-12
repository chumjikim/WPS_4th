day 1 

1. 파이썬 인터프리터를 계산기처럼 사용해서 1년이 총 몇 초인지 계산해보시오.
  * In [158]: 60*60*24*365
  * Out[158]: 31536000

2. 1일이 몇 초인지 계산 후, 해당 결과를 seconds_per_day 변수에 할당하라.
 * In [159]: seconds_per_day=60*60*24

 * In [160]: seconds_per_day
 * Out[160]: 86400
3. 1년이 몇 초인지 계산 후, 해당 결과를 seconds_per_year 변수에 할당하라.
 * In [161]: seconds_per_year=60*60*24*365
 * In [162]: seconds_per_year
 * Out[162]: 31536000
4. 문자열을 입력해주세요 :라는 안내문구를 띄워주도록 input함수를 사용해본다. 결과는 var에 할당한다.
 * In [165]: var=input('문자열을 입력해주세요 : ') 문자열을 입력해주세요 :  hello
 * In [166]: var
 * Out[166]: 'hello'

5. 양수 1234를 표현해본다.
 * In [167]: 1234
 * Out[167]: 1234
6. 음수 100을 표현해본다.
 * In [168]: -100
 * Out[168]: -100
7. 10과 3의 덧셈, 뺄셈, 곱셈, 나눗셈, 정수나눗셈, 나머지, 지수 연산을 실행해본다.
 * In [169]: 10+3
 * Out[169]: 13

 * In [170]: 10-3
 * Out[170]: 7

 * In [171]: 10/3
 * Out[171]: 3.3333333333333335

 * In [172]: 10//3
 * Out[172]: 3

 * In [174]: 10%3
 * Out[174]: 1

 * In [175]: 10**3
 * Out[175]: 1000

8. c에 60을 할당 후, 산술 연산자 결합으로 아래 목록을 표현해본다.

 * In [177]: c+=1

 * In [178]: c/=1

 * In [179]: c%=1

9. 부동소수점수형 변수를 정수형으로 형변환 해본다.
 * In [181]: int(3.14)
 * Out[181]: 3


10. 정수형 변수를 부동소수점형으로 형변환 해본다.
 * In [182]: float(3)
 * Out[182]: 3.0
 
11. 여러 줄의 텍스트를 multi_lines변수에 할당하고, print()함수로의 출력과 인터프리터의 자동 출력(변수명 입력)을 비교해보시오.
 * In [191]: In [234]: multi_lines='''  
     ...:      ...: asdf  
     ...:      ...: asdf  
     ...:      ...: asdf  
     ...:      ...: '''   

 * In [192]: multi_lines
 * Out[192]: '\nasdf\nasdf\nasdf\n'

 * In [193]: print(multi_lines).  

   asdf  
   asdf  
   asdf  


12. str1, str2변수에 각각 문자열을 할당하고, 두 변수를 결합해 str3변수에 할당해보시오.

 * In [198]: str3 = ''

 * In [199]: str3+=str1

 * In [200]: str3+=str2

 * In [201]: str3
 * Out[201]: 'hi hello'


13. str1변수에 *연산자를 사용한 결과를 출력해보시오.

 * In [202]: str1*3
 * Out[202]: 'hihihi'


14. sample_list2를 이용해서 실습. 5월, 7월을 인덱스연산을 통해 추출해보자.
 * In [205]: sample_list2[::3]
 * Out[205]: ['Jan', 'Apr', 'Jul', 'Oct']

15. sample_list를 이용, 3번째 요소인 'c'를 대문자 'C'로 바꿔본다.
 * In [206]: sample_list2[2]='C'

 * In [207]: sample_list2[2]
 * Out[207]: 'C'

16. 슬라이스 연산
 * sample_list2를 이용, 1월부터 3월씩 건너뛴 결과를 quarters에 할당
  * In [208]: sample_list2[::3]
  * Out[208]: ['Jan', 'Apr', 'Jul', 'Oct']
 * sample_list2를 이용, 끝에서부터 3번째 요소까지를 last_three에 할당
  * In [210]: sample_list2[-1:-4:-1]
  * Out[210]: ['Dec', 'Nov', 'Oct']
 * sample_list2를 이용, 끝에서부터 처음까지(거꾸로) 2월씩 건너뛴 결과를 reverse_two_steps에 할당
  * In [216]: sample_list2[-1::-2]
  * Out[216]: ['Dec', 'Oct', 'Aug', 'Jun', 'Apr', 'Feb'] 
 * 
17. 리스트 함수 insert(offset)을 사용

 - fruits리스트의 1번째 위치에 'mango'를 추가해보자
  * In [219]: fruits.insert(1,'mango')
 * fruits리스트의 100번째 위치에 'pineapple'을 추가해보자
  * In [220]: fruits.insert(100,'pineapple')


18.  문자열 'Fastcampus'를 리스트, 튜플 타입으로 형변환하여 새 변수에 할당한다.
 * In [250]: fast.split(',')
 * Out[250]: ['fastcampus']

 * In [253]: ','.join(fast)
 * Out[253]: 'f,a,s,t,c,a,m,p,u,s'

19. 1번에서 할당한 리스트, 튜플 변수를 이용해 다시 문자열을 만든다.
 
 
20. 6가지 색상의 영어 키, 한글 값을 갖는 딕셔너리(colors)를 생성한다. (red, green, blue, yellow, black, white)
colors에서 blue키의 값을 출력한다.

 * In [436]: colors['blue']
 * Out[436]: '파랑'

21. colors를 셋(Set)으로 만들어 colors_set변수에 할당한다.
 * In [271]: colors_set=set(colors)

 * In [272]: colors_set
 * Out[272]: {'black', 'blue', 'green', 'red', 'white', 'yellow'}

22. colors_set에 purple이 존재하는지 확인한다.

 * In [273]: 'purple' in colors_set
 * Out[273]: False

23. [2,4,3,7,6,8,4,6,5,3,2,5,6,7,3]에서 중복된 값을 없애고, 오름차순으로 정렬한 list를 반환한다.
 * In [274]: list_num = [2,4,3,7,6,8,4,6,5,3,2,5,6,7,3]

 * In [275]: set(list_num)
 * Out[275]: {2, 3, 4, 5, 6, 7, 8}

24. 2차원 딕셔너리인 lol을 만든다.
lol딕셔너리의 champions키에 셋(Set)을 이용해 lux, ahri, ezreal을 할당하고,
lol딕셔너리의 items키에 아래의 각 항목들을 딕셔너리를 이용해 리스트로 할당한다.

* Key: Doran Ring, Value: 400


* Key: Doran Blade, Value: 450


25. x = {1,2,3,4,5,6,8}, y={4,5,6,9,10,11}, z={4,6,8,9,7,10,12}일 때,

* x,y,z의 교집합에 해당하는 숫자는?
* y,z의 교집합이며 x에는 속하지 않는 숫자는?
* x에만 속하고 y,z에는 속하지 않는 숫자는?