

### 문제 1:평균 일일 대여 요금 구하기
```sql
SELECT ROUND(AVG(DAILY_FEE), 0) AS AVERAGE_FEE
FROM CAR_RENTAL_COMPANY_CAR
WHERE CAR_TYPE = 'SUV'
```
내장함수 `ROUND(대상, 숫자)` `대상`을 소수점 아래 `숫자` 자리까지 반올림
### 문제2: 조건에 맞는 도서 리스트 출력하기
![[Pasted image 20240206174032.png]]
를 활용한다
```SQL
SELECT BOOK_ID, DATE_FORMAT(PUBLISHED_DATE,"%Y-%m-%d") AS PUBLISHED_DATE
FROM BOOK
WHERE YEAR(PUBLISHED_DATE) = 2021 AND CATEGORY='인문'
```

### 문제 5: 과일로 만든 아이스크림 고르기
```SQL
-- 코드를 입력하세요
SELECT A.FLAVOR
FROM 
FIRST_HALF AS A
LEFT JOIN ICECREAM_INFO AS B
ON A.FLAVOR = B.FLAVOR
WHERE A.TOTAL_ORDER > 3000 AND B.INGREDIENT_TYPE = 'fruit_based'
ORDER BY A.TOTAL_ORDER DESC;
```
- 2개를 조인시키고 특정 기준을 WHERE에 넣으면 된다.

### 문제 6: 강원도에 위치한 생산공장 목록 출력하기
```SQL
SELECT FACTORY_ID, FACTORY_NAME, ADDRESS
FROM FOOD_FACTORY 
WHERE ADDRESS LIKE '강원도%';
```
- ADDRESS에서 강원도로 시작하는 걸 찾으면 됐는데, LIKE, %를 붙이면 된다. [[LIKE]]는 언젠가 정리할 것

### 문제 9: 조건에 부합하는 중고거래 댓글 조회하기

```SQL
SELECT A.TITLE,
A.BOARD_ID,
B.REPLY_ID,
B.WRITER_ID,
B.CONTENTS,
DATE_FORMAT(B.CREATED_DATE, '%Y-%m-%d') AS CREATED_DATE
FROM 
USED_GOODS_BOARD AS A
INNER JOIN
USED_GOODS_REPLY AS B
ON A.BOARD_ID = B.BOARD_ID
WHERE B.CREATED_DATE LIKE '2022-10%'
ORDER BY B.CREATED_DATE ASC, A.TITLE ASC;
```
- 이렇게 했는데 에러가 난다 어떻게 고쳐야 하는가? 
	- 문제를 제대로 안읽었었다. 댓글 작성 일자가 10월이 아니라, 게시물 작성 시가 10월이었따. 그래서
```SQL
WHERE B.CREATED_DATE LIKE '2022-10%'
```
을
```SQL
WHERE A.CREATED_DATE LIKE '2022-10%'
```
로 바꾸니 정답.

### 문제 10: 서울에 위치한 식당 목록 출력하기
- 서울에 있는 식당의 별점 평균을 내야하는데 어떻게 내야하는지 모름
	- AVG() 내장함수로 평균값 내면 된다고 하고
	- 그룹 바이로 묶으면 된다.
```SQL
-- 코드를 입력하세요
SELECT  B.REST_ID
        ,A.REST_NAME
        ,A.FOOD_TYPE
        ,A.FAVORITES
        ,A.ADDRESS
        ,ROUND(AVG(B.REVIEW_SCORE),2) AS SCORE
FROM
    REST_REVIEW  AS B
JOIN 
    REST_INFO AS A
ON A.REST_ID = B.REST_ID
GROUP BY B.REST_ID
HAVING A.ADDRESS LIKE '서울%'
ORDER BY SCORE DESC, FAVORITES DESC;
```
위 문제를 풀다가 궁금해진점
### HAVING과 WHERE의 차이점이 뭐지?
둘 다 기능은 같은데, WHERE은 그룹화 되기 전에 적용, HAVING은 그룹화 된 후 적용

### 문제 11: 모든 레코드 조회하기
- 걍 ORDER ~ BY ASC 하면 끝
### 문제 12: 오프라인/온라인 판매 데이터 통합하기
- 데이터를 PRODUCT_ID 기준으로 ONLINE_SALE에 LEFT JOIN 시키고, SALES_AMOUNT는 만약 같으면 둘이 더해주기 위해 ADD 함수를 사용했다.
	- 하지만 오답
```SQL
-- 코드를 입력하세요
SELECT DATE_FORMAT(A.SALES_DATE, 'Y%-M%-D%') AS SALES_DATE,
    A.PRODUCT_ID,
    A.USER_ID,
    ADD(A.SALES_AMOUNT,B.SALES_AMOUNT) AS SALES_AMOUNT
FROM 
ONLINE_SALE  AS A
LEFT JOIN 
OFFLINE_SALE AS B
ON A.PRODUCT_ID = B.PRODUCT_ID
WHERE SALES_DATE LIKE '2022-03%'
ORDER BY SALES_DATE ASC, PRODUCT_ID ASC, PRODUCT_ID ASC;
```
- 찾아보니 UNION을 사용해서 합쳐야 했다. 다시 해보자
- 각자 형태가 다른 두 테이블을 하나의 형식으로 바꿔서 합춰줘야 하니 UNION을 쓰는것이 편하다.( USER_ID를 NULL로 변하게 하는것과 같은 작업을 할때 유용하다)
```SQL
-- 코드를 입력하세요
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d'),
    PRODUCT_ID,
    USER_ID,
    SALES_AMOUNT
FROM ONLINE_SALE 
WHERE SALES_DATE LIKE '2022-03%'
UNION
SELECT  DATE_FORMAT(SALES_DATE, '%Y-%m-%d'),
    PRODUCT_ID,
    NULL AS USER_ID,
    SALES_AMOUNT
FROM OFFLINE_SALE
WHERE SALES_DATE LIKE '2022-03%'
ORDER BY SALES_DATE ASC, PRODUCT_ID ASC, USER_ID ASC;

```
이렇게 썼는데 오류가 났다. `Unknown column 'SALES_DATE' in 'order clause'`
```SQL
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d'),
```
부분에서SALES_DATE를 따로 설정해주지 않았기 때문.
```SQL
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE,
```
이렇게 바꿔주었다
```SQL
-- 코드를 입력하세요
SELECT DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE,
    PRODUCT_ID,
    USER_ID,
    SALES_AMOUNT
FROM ONLINE_SALE 
WHERE SALES_DATE LIKE '2022-03%'
UNION
SELECT  DATE_FORMAT(SALES_DATE, '%Y-%m-%d') AS SALES_DATE,
    PRODUCT_ID,
    NULL AS USER_ID,
    SALES_AMOUNT
FROM OFFLINE_SALE
WHERE SALES_DATE LIKE '2022-03%'
ORDER BY SALES_DATE ASC, PRODUCT_ID ASC, USER_ID ASC;
```
- 정답 코드
### 21번: 1. 조건에 맞는 회원수 구하기

- [문제 링크](https://school.programmers.co.kr/learn/courses/30/lessons/131535#)
- 부합하는 row 수를 어떻게 셀지 몰랐는데
```SQL
-- 코드를 입력하세요
SELECT COUNT(*) AS USERS
FROM USER_INFO 
WHERE JOINED LIKE '2021-%' AND AGE >= 20 AND AGE <= 29;
```
`SELECT COUNT(*) AS ~~`를 써주면 된다.