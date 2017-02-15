## SQL Intro

* SQL은 데이터베이스를 액세스하고 조작하기위한 표준 언어입니다.

###SQL이란?
* SQL은 구조화 된 쿼리 언어
* SQL을 사용하면 데이터베이스에 액세스하고 조작 할 수 있습니다.
* SQL은 ANSI (American National Standards Institute) 표준입니다.


## SQL Syntax
데이터베이스에서 수행해야하는 대부분의 조치는 SQL 문으로 완료됩니다.
## 1. SQL Select
데이터베이스에서 데이터를 선택하는데 사용된다. 결과는 결과세트라고 하는 결과 테이블에 저장된다.

사용방법
>SELECT column_name,column_name
FROM table_name;

## 2. SQL Distinct
테이블에서 열은 많은 중복값을 포함할 수 있다.
중복되는 값을 제외하고 불러올 때 사용한다.

사용방법
> SELECT DISTINCT column_name,column_name
FROM table_name;

## 3. SQL Where
지정된 기준을 충족하는 레코드만 불러와서 보여준다.

사용방법
>SELECT column_name,column_name
FROM table_name
WHERE column_name operator value;


## 4. SQL And & Or
둘 이상의 조건에 따라 레코드를 필터링하는데 사용된다.

AND - 첫 번째 조건과 두번째 조건이 모두 참일 경우의 레코드를 보여준다
OR - 첫 번째 또는 두번째 조건이 참일 경우 레코드를 보여준다

사용방법
>SELECT * FROM Customers
WHERE Country='Germany'
AND City='Berlin';
OR City='München';

## 5. SQL Order By
하나 이상의 열을 기준으로 결과 집합을 정렬하는데 사용된다.
기본적으로 오름차순으로 정렬한다.
내림차순으로 정렬하려면 DESC키워드를 사용한다.

사용방법
>SELECT column_name, column_name
FROM table_name
ORDER BY column_name ASC|DESC, column_name ASC|DESC;


## 6. SQL Insert Into
테이블에 새 레코드를 삽입하는데에 사용한다

사용방법
>INSERT INTO table_name
VALUES (value1,value2,value3,...);


## 7. SQL Update
기존 레코드를 업데이트하는데 사용된다.

사용방법
>UPDATE Customers
SET City='Hamburg'
WHERE CustomerID=1;

## 8. SQL Delete
테이블의 레코드를 삭제하는 데 사용한다

사용방법
>DELETE FROM table_name
WHERE some_column=some_value;


## SQL Like
LIKE 연산자는 WHERE 절에서 열의 지정된 패턴을 검색하는 데 사용된다.

사용방법 
(도시가 문자 "s"로 시작하는 모든 고객을 선택)
> SELECT * FROM Customers
WHERE City LIKE 's%';

## 주요 명령어 정리
* SELECT - 데이터를 추출
* UPDATE - 데이터를 업데이트
* DELETE - 데이터를 삭제
* INSERT INTO - 새로운 데이터를 삽입

* CREATE DATABASE - 새 데이터베이스 생성
* ALTER DATABASE - 데이터베이스를 수정

* CREATE TABLE - 새로운 테이블 수정
* ALTER TABLE - 테이블을 수정
* DROP TABLE - 테이블 삭제

* CREATE INDEX - 인덱스(검색 키)를 생성
* DROP INDEX - 인덱스를 삭제

