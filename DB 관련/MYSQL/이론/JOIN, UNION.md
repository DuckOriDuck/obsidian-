테이블 여러개를 한번에 조회

# JOIN
- 2개 이상의 테이블을 엮어야 할때 사용 
- 기본적으로 JOIN을 시전하기 위해서는 테이블을 연결해주는 KEY가 필요.(공통 열)

## INNER JOIN
- 두 테이블을 연결할 때 많이 사용하는 것
```sql
SELECT *
FROM students AS A
	INNER JOIN classes AS B
	ON A.name = B.name;
```
- 모든 열을 선택
- INNER JOIN 전 후로 명시된 두 개의 테이블을 조인함.
- ON 구문에는 어떤 열을 기준으로 INNER JOIN을 수행할 지 조건을 적어줌. name 열을 기준으로 INNER JOIN을 수행해줌
![[Pasted image 20240206171921.png]]

## FULL OUTER JOIN
FULL OUTER JOIN을 활용해서 student 테이블과 classes 테이블을 조회하는 SQL문
```sql
SELECT *
FROM students AS A
	FULL OUTER JOIN classes AS B
	ON A.name = B.name;
```

INNER JOIN은 공통된 데이터만 가져온다면 OUTER JOIN은 모든 정보를 가져온다.
![[Pasted image 20240206172350.png]]
OUTER JOIN의 종류

## LEFT OUTER JOIN
```sql
SELECT [열 목록]
FROM [LEFT 테이블] LEFT OUTER JOIN [RIGHT 테이블]
			ON [조인 조건]
[WHERE 검색 조건]
```
왼쪽의 테이블의 행만 가져오는데, B 테이블과 검색조건에 맞는 행이 있다면 B 테이블에만 있는 열의 정보를 추가해준다. B와 겹치지 않는 A의 행들에게는 NULL이 추가된다.

## RIGHT OUTER JOIN
```sql
SELECT [열 목록]
FROM [LEFT 테이블] RIGHT OUTER JOIN [RIGHT 테이블]
			ON [조인 조건]
[WHERE 검색 조건]
```
위와 동일
# UNION
이란? 2개의 SQL 실행 결과를 결합하는데 사용
JOIN과 다르게 UNION은 테이블 사이의 관계성이 없어도 사용 가능
	JOIN은 공통된 열을 기준으로 양옆으로 붙인다면 UNION은 양옆으로 붙임
## 예제
SQL 실행 결과를 하나로 이어붙이는 형태
```sql
[SQL 1]
UNION
[SQL 2];
```
### UNION으로 결합하기 위해서 필요한 조건
- SQL1과 SQL2의 열 개수가 같아야 함
- SQL1과 SQL2의 열 이름이 같아야 함 
- SQL1과 SQL2의 각 열의 데이터 타입이 동일해야 함
		그냥 2개 합쳐야 하니깐 열이 아예 동일해야 함.
학생정보로 UNION 결합하는 SQL문. 중복되는 행이 있다면 없애줌.
```sql
SELECT name, age FROM students WHERE age < 30
UNION
SELECT name, age FROM students WHERE age < 32
```
## UNION FULL
```sql
[SQL 1]
UNION ALL
[SQL 2];
```
UNION ALL은 중복 제거 없이 모두 보여준다.