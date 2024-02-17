SELECT의 기본 개념:

| SELECT | 조회할 열을 지정 |
| ---- | ---- |
| FROM | 조회할 테이블을 지정 |
| WHERE | 조회할 데이터를 필터링 |

``` mySQL
SELECT *
FROM students


SELECT name AS col1, age AS col2
FROM students

SELECT name, age
FROM students
WHERE age>=30

```
- `*`를 통해 와일드카드 선언 가능
- 선택해오는 열의 이름을 따로 설정 가능
- WHERE 의 조건문을 통하여 사용 가능,
	-  WHERE의  조건문에 사용되는  비교 연산자:
		- = (같음)
		- >(보다 큼)
		- >= (보다 크거나 같음)
		- < (보다 작음)
		- <= (보다 작거나 같음)
		- <> , != , ^= (같지 않음)
- varchar형식은 `' '`로 감싸야 한다.