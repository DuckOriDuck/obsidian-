### 1: 즐겨찾기가 가장 많은 식당 정보 출력하기
찾아본 이유:
한식, 일식, 양식 이런거 종류별로 나눈다음에, 가장 FAVORITES가 많은 식당만을 골라야 했는데, 어떻게 해야 할지를 몰랐다, GROUP BY는 그냥 특정 행 기준으로 해버리면 다 합쳐버리는거 아닌가?
근데 보니깐 [[서브쿼리]]를 써야 하더라
```SQL
-- 코드를 입력하세요
SELECT FOOD_TYPE, REST_ID, REST_NAME, FAVORITES
FROM REST_INFO
WHERE(FOOD_TYPE, FAVORITES)
IN
(SELECT FOOD_TYPE, MAX(FAVORITES)
FROM REST_INFO
GROUP BY FOOD_TYPE)

ORDER BY FOOD_TYPE DESC
```
이렇게 한다고 한다
- SQL에서 MAX(), MIN() 구문 -> 가장 크기가 크거나 작은 수를 가져온다.
- `SELECT FOOD_TYPE, MAX(FAVORITES)`에서 FOOD TYPE의 FAVOTIES 값을 모두 MAX로 만들어버리고, FOD TYPE으로 GROUP 해버리니 하나의 행밖에 남지 않는다. 최고값을 가진.
- 그래서 최고값을 가진 해당 음식 종류의 음식점을 찾으니 되는것
## [2. 조건에 맞는 사용자와 총 거래금액 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164668)

```SQL
SELECT B.USER_ID , B.NICKNAME, SUM(A.PRICE) AS TOTAL_SALES
FROM 
USED_GOODS_BOARD AS A
JOIN
USED_GOODS_USER AS B
ON B.USER_ID = A.WRITER_ID
WHERE A.STATUS = 'DONE'
GROUP BY B.USER_ID
HAVING TOTAL_SALES >= 700000
ORDER BY TOTAL_SALES;
```
이거라고 한다
### [3. 저자 별 카테고리 별 매출액 집계하기](https://school.programmers.co.kr/learn/courses/30/lessons/144856)

열`*`열을 어떻게 해야하지? 심지어 테이블이 3개인 상황
- `BOOK.PUBLISHED_DATE` 이2022년 1월이면서 
- 저자 별, 카테고리 별 매출액을 구해야한다.
```SQL
-- 코드를 입력하세요
SELECT A.AUTHOR_ID, B.AUTHOR_NAME, A.CATEGORY, SUM(A.PRICE*C.SALES) AS TOTAL_SALES
from 
BOOK A 
JOIN AUTHOR B ON A.AUTHOR_ID = B.AUTHOR_ID
JOIN BOOK_SALES  C ON A.BOOK_ID = C.BOOK_ID
where C.SALES_DATE LIKE '2022-01%'
GROUP BY AUTHOR_ID, CATEGORY
ORDER BY A.AUTHOR_ID ASC, A.CATEGORY DESC
```
- 놀랍게도 JOIN을 여러 번 쓸 수 있다는 사실 
- GROUP BY 행 2개를 쓰면 그 2개를 기준으로 알아서 잘 나눠준다. 
- `SUM(A.PRICE*C.SALES) AS TOTAL_SALES`이 부분이 어떻게 먹히는지는 아직 잘 이해가 안됨

### [4. 카테고리 별 도서 판매량 집계하기](https://school.programmers.co.kr/learn/courses/30/lessons/144855)
```SQL
-- 코드를 입력하세요
SELECT A.CATEGORY, SUM(B.SALES) AS TOTAL_PRICE
FROM BOOK A 
JOIN BOOK_SALES B ON A.BOOK_ID = B.BOOK_ID
WHERE B.SALES_DATE LIKE '2022-01-%' 
GROUP BY CATEGORY
ORDER BY CATEGORY;
```
이렇..게 하면 된다고 한다?

### [5. 자동차 대여 기록에서 대여중 / 대여 가능 여부 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/157340)
```SQL
-- 코드를 입력하세요
SELECT car_id, max(if('2022-10-16' between start_date and end_date,'대여중','대여 가능')) as AVAILABILITY
from car_rental_company_rental_history
GROUP BY CAR_ID
ORDER BY CAR_ID DESC;
```

### [6. 자동차 종류 별 특정 옵션이 포함된 자동차 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151137)
```SQL
-- 코드를 입력하세요
SELECT CAR_TYPE, COUNT(CAR_TYPE) AS CARS
FROM CAR_RENTAL_COMPANY_CAR
WHERE OPTIONS LIKE '%통풍시트%'OR OPTIONS LIKE '%열선시트%'OR OPTIONS LIKE '%가죽시트%'
GROUP BY CAR_TYPE
ORDER BY CAR_TYPE ASC;
```

### [7.대여 횟수가 많은 자동차들의 월별 대여 횟수 구하기 ](https://school.programmers.co.kr/learn/courses/30/lessons/151139)
```SQL
-- 코드를 입력하세요
SELECT MONTH(START_DATE) AS MONTH, CAR_ID, COUNT(CAR_ID) AS RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY 
WHERE START_DATE<='2022-10-31' AND START_DATE>='2022-08-01'
GROUP BY MONTH, CAR_ID 
HAVING RECORDS >=5
ORDER BY MONTH ASC , CAR_ID DESC;
```
했는데 오답이라고 한다. 답을 찾아보았다.`
```SQL
-- 코드를 입력하세요
SELECT MONTH(START_DATE) AS MONTH, CAR_ID, COUNT(*) AS RECORDS
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY 
WHERE DATE_FORMAT(START_DATE, '%Y-%m') BETWEEN '2022-08' AND '2022-10'
        AND CAR_ID IN(SELECT CAR_ID
                     FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
                     WHERE DATE_FORMAT(START_DATE, '%Y-%m') BETWEEN '2022-08' AND '2022-10'
                     GROUP BY CAR_ID
                     HAVING COUNT(CAR_ID)>=5)
GROUP BY MONTH(START_DATE), CAR_ID 
HAVING RECORDS >=1
ORDER BY MONTH ASC , CAR_ID DESC;
```
- 이게 맞다고 한다.
- 8월~10월 사이으 ㅣ대여횟수가 총 5회 이상인걸 월별, 차ID별 횟수를 알려달라고 했으니 이렇게 서브쿼리를 쓰는게 맞다.
- 첫 번째 쿼리대로 쓰면 월별, ID별로 나눠지긴 하는데 RECORDS가 월별로 나눠져서 나오지 않고, 8~10월 간에 빌린 횟수만큼 출력돼버린다.
### [9 성분으로 구분한 아이스크림 총 주문량](https://school.programmers.co.kr/learn/courses/30/lessons/133026)
```SQL
SELECT B.INGREDIENT_TYPE, SUM(A.TOTAL_ORDER) AS TOTAL_ORDER
FROM FIRST_HALF A
JOIN ICECREAM_INFO B ON A.FLAVOR = B.FLAVOR
GROUP BY B.INGREDIENT_TYPE
ORDER BY TOTAL_ORDER;
```
GROUP BY 하고 SUM 해버리면 합쳐지는 애들끼리 알아서 더해진다.
### [10.식품분류별 가장 비싼 식품의 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131116)
```SQL
-- 코드를 입력하세요
SELECT CATEGORY, MAX(PRICE) AS MAX_PRICE, PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE CATEGORY IN ('과자', '국', '김치', '식용유')
GROUP BY CATEGORY
ORDER BY MAX_PRICE DESC;
```
왜 틀린거지?
```SQL
-- 코드를 입력하세요
SELECT CATEGORY, PRICE AS MAX_PRICE, PRODUCT_NAME
FROM FOOD_PRODUCT
WHERE PRICE IN (SELECT MAX(PRICE) FROM FOOD_PRODUCT GROUP BY CATEGORY)
            AND CATEGORY IN ('과자', '국', '김치', '식용유')
GROUP BY CATEGORY
ORDER BY MAX_PRICE DESC;
```
정답 찾아보니 이렇게 하라는데,
지피티한테 물어보니, 
- "`SELECT` 절에 `PRODUCT_NAME`이 포함되어 있는데, 이는 `GROUP BY` 절에 나열되지 않았습니다. SQL 표준에 따르면, `GROUP BY` 절에 나열되지 않은 모든 컬럼은 집계 함수 내에서 사용되어야 합니다."
	- 그니깐,  PRODUCT_NAME을 집계함수인 `MAX()`에도, `GROUP BY`에도 안 써서 오류가 났다는 것. 
	- 여러 행을 포함시키는데 한 행에 대해서 MAX()나 SUM()이나 AVG()같은 집계함수 값을 써서 조건을 완성시켜야 한다면, 저렇게 서브쿼리를 쓰는게 맞는 것이다.

### [11.고양이와 개는 몇 마리 있을까 ]( https://school.programmers.co.kr/learn/courses/30/lessons/59040)
"고양이를 개보다 먼저 조회해주세요." 이걸 어떻게 구현하는지 모르겠어서 일단은 찾아봄 근데 뭐 푼게 맞았네.

### [13:년, 월, 성별 별 상품 구매 회원 수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131532)
```SQL
-- 코드를 입력하세요
SELECT YEAR(B.SALES_DATE) AS YEAR
    , MONTH(B.SALES_DATE) AS MONTH
    , A.GENDER
    , COUNT(A.USER_ID) AS USERS
FROM 
USER_INFO A
JOIN 
ONLINE_SALE B ON A.USER_ID = B.USER_ID
GROUP BY YEAR AND MONTH AND GENDER
HAVING GENDER IS NOT NULL
ORDER BY YEAR, MONTH, GENDER
```
이게 왜안됨!!
```SQL
-- 코드를 입력하세요
SELECT YEAR(B.SALES_DATE) AS YEAR
    , MONTH(B.SALES_DATE) AS MONTH
    , A.GENDER
    , COUNT(DISTINCT A.USER_ID) AS USERS
FROM 
USER_INFO A
JOIN 
ONLINE_SALE B ON A.USER_ID = B.USER_ID
WHERE GENDER IS NOT NULL
GROUP BY YEAR, MONTH, GENDER
ORDER BY YEAR, MONTH, GENDER
```
그룹바이를 고치니 됐다..? 
***그룹바이에서는 AND 쓰는게 아니라고 한다.*** 그룹바이 빼곤 다 잘 한거였음././
### [14. 입양 시각 구하기(1)](https://school.programmers.co.kr/learn/courses/30/lessons/59412)
```SQL
-- 코드를 입력하세요
SELECT HOUR(DATETIME) AS HOUR, COUNT(ANIMAL_ID) AS COUNT
FROM ANIMAL_OUTS
WHERE TIME_FORMAT(DATETIME, '%H:%m') BETWEEN '09:00' AND '19:59'
GROUP BY HOUR
ORDER BY HOUR
```
직접 한거 뿌듯해서 넣어봄

### [14. 입양 시각 구하기(2)](https://school.programmers.co.kr/learn/courses/30/lessons/59413)
```SQL
-- 코드를 입력하세요
SELECT HOUR(DATETIME) AS HOUR, COUNT(ANIMAL_ID) AS COUNT
FROM ANIMAL_OUTS
GROUP BY HOUR
ORDER BY HOUR;
```
	-  이렇게 쓰니깐 COUNT가 0인 테이블은 안떴다. 0인 테이블도 떠서 0~23시까지 뜨게 하려면 어떻게 해야하지? 찾아봄.
- SET을 써야함. SET은 어떤 변수에 특정 값을 할당할때 쓰는 명령어이다.

```SQL
SET @HOUR = -1;
SELECT (@HOUR := @HOUR +1) AS HOUR
FROM ANIMAL_OUTS
WHERE @HOUR < 23;
```

```SQL
-- 코드를 입력하세요
SET @HOUR =-1;
SELECT (@HOUR := @HOUR +1) AS HOUR, 
        (SELECT COUNT(HOUR(DATETIME))
         FROM ANIMAL_OUTS
         WHERE HOUR(DATETIME) = @HOUR) AS COUNT
FROM ANIMAL_OUTS
WHERE @HOUR<23;
```
솔직히 코드 봐도 모르겠다... 해설을 보니
	- HOUR 변수를 -1로 선언하여서 22까지 +1씩 더해주었다. -1부터 더했기 때문에 0부터 23까지 생성이 된다.
	- ANIMAL_OUTS 테이블에 있는 DATETIME 변수와 @HOUR 변수가 동일한 순간 카운트를 진행한다.
- 라고 한다...
#### [참고 블로그](https://chanhuiseok.github.io/posts/db-6/) 
### [16.가격대 별 상품 개수 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131530)
```SQL
-- 코드를 입력하세요
SELECT FLOOR(PRICE / 10000) *10000 AS PRICE_GROUP, COUNT(PRODUCT_ID) AS PRODUCTS
FROM PRODUCT 
GROUP BY PRICE_GROUP
ORDER BY PRICE_GROUP;
```
소수점 절사 함수 FLOOR를 쓰면 된다.

