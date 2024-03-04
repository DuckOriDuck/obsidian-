### [1. 자동차 대여 기록에서 장기/단기 구분하기](https://school.programmers.co.kr/learn/courses/30/lessons/151138)
day-day가 가능한것인가? 궁금해서 찾아봄
Chat gpt:
*MySQL에서는 `DATEDIFF()` 함수를 사용하여 두 날짜 간의 차이를 일 단위로 계산할 수 있습니다. 예를 들어, `END_DATE`와 `START_DATE`의 차이를 계산하려면 `DATEDIFF(END_DATE, START_DATE)`를 사용합니다.*
 ```SQL
 -- 코드를 입력하세요
SELECT HISTORY_ID,
        CAR_ID,
        DATE_FORMAT(START_DATE,'%Y-%m-%d') AS START_DATE,
        DATE_FORMAT(END_DATE,'%Y-%m-%d') AS END_DATE,
        CASE WHEN DATEDIFF(END_DATE,START_DATE) >=30 THEN'장기 대여'
        ELSE '단기 대여' END AS RENT_TYPE
FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY 
WHERE START_DATE LIKE '2022-09-%'
ORDER BY HISTORY_ID DESC;
```
이게 왜틀린지 모르겠음. 문제 오류였음.
### [2.조회수가 가장 많은 중고거래 게시판의 첨부파일 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164671)
여러개의 STRING을 어떻게 붙여야할지 모르겠다. CONCAT을 붙여야하나?
### 1. CONCAT 함수 사용:

대부분의 SQL 데이터베이스 시스템에서 `CONCAT` 함수를 사용하여 여러 문자열을 결합할 수 있습니다. 예를 들어:

```SQL
SELECT CONCAT(string1, string2, string3) AS combined_string
FROM your_table;
```

이 예에서 `string1`, `string2`, `string3`은 결합하고자 하는 문자열 필드나 문자열 리터럴입니다.
### 2. MySQL의 CONCAT_WS 함수 사용:

MySQL에서는 `CONCAT_WS` (Concatenate With Separator) 함수를 사용하여 구분자와 함께 여러 문자열을 결합할 수 있습니다. 이 함수는 여러 문자열 사이에 일관된 구분자를 추가하는 데 유용합니다.

```SQL
SELECT CONCAT_WS('-', string1, string2, string3) AS combined_string
FROM your_table;
```
이 예에서는 문자열 사이에 하이픈(`-`)을 구분자로 사용합니다.
라고 한다.
그리고 가장 큰 VIEWS를 가진 ROW만 어떻게 챙겨오는지 기억이 나지 않는다. 다시 한번 물어본다
```SQL
(SELECT MAX(VIEWS) FROM USED_GOODS_BOARD)
```
를 WHERE문에 씀녇 ㅚㄴ다고 한다.
```SQL
-- 코드를 입력하세요
SELECT CONCAT('/home/grep/src/', A.BOARD_ID,'/',B.FILE_ID, B.FILE_NAME, B.FILE_EXT)AS FILE_PATH
FROM USED_GOODS_BOARD A
JOIN USED_GOODS_FILE B 
ON A.BOARD_ID = B.BOARD_ID
WHERE A.VIEWS = (SELECT MAX(VIEWS) FROM USED_GOODS_BOARD )
ORDER BY B.FILE_ID DESC;
```
이렇게 쓰니 됐다

### [3. 자동차 대여 기록 별 대여 금액 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/151141)
```SQL
-- 코드를 입력하세요
WITH RENTAL_HISTORY AS(
	SELECT *,
	        DATEDIFF(END_DATE, START_DATE) AS DATE_DIFF,
			CASE 
				WHEN DATEDIFF(END_DATE, START_DATE)>=90 THEN '90일 이상'
	            WHEN DATEDIFF(END_DATE, START_DATE)>=30 THEN '30일 이상'
	            WHEN DATEDIFF(END_DATE, START_DATE)>=7 THEN '7일 이상'
	            ELSE NULL END AS DURATION_TYPE
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
)

SELECT H.HISTORY_ID, FLOOR(C.DAILY_FEE* H.DATE_DIFF * COALESCE((100-P.DISCOUNT_RATE)/100,1))AS FEE

FROM RENTAL_HISTORY H

LEFT JOIN CAR_RENTAL_COMPANY_CAR C 
ON H.CAR_ID = C.CAR_ID

LEFT JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN P
ON C.CAR_TYPE = P.CAR_TYPE AND H.DURATION_TYPE = P.DURATION_TYPE

WHERE C.CAR_TYPE = '트럭'

ORDER BY FEE DESC, H.HISTORY_ID DESC

```
했는데 오답이 나온다. 문제를 찾아보자.
```SQL
WITH RENTAL_HISTORY AS (
    SELECT *,
           DATEDIFF(END_DATE, START_DATE)+1 AS DATE_DIFF,
           CASE 
               WHEN DATEDIFF(END_DATE, START_DATE) >= 90 THEN '90일 이상'
               WHEN DATEDIFF(END_DATE, START_DATE) >= 30 THEN '30일 이상'
               WHEN DATEDIFF(END_DATE, START_DATE) >= 7 THEN '7일 이상'
               ELSE NULL END AS DURATION_TYPE
    FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
)

SELECT H.HISTORY_ID, 
       FLOOR(C.DAILY_FEE * H.DATE_DIFF * (100 - COALESCE(P.DISCOUNT_RATE,0)) / 100) AS FEE
FROM RENTAL_HISTORY H
JOIN CAR_RENTAL_COMPANY_CAR C ON H.CAR_ID = C.CAR_ID
LEFT JOIN CAR_RENTAL_COMPANY_DISCOUNT_PLAN P ON C.CAR_TYPE = P.CAR_TYPE AND H.DURATION_TYPE = P.DURATION_TYPE
WHERE C.CAR_TYPE = '트럭'
ORDER BY FEE DESC, H.HISTORY_ID DESC;
```
차를 하루만 빌리면 DAILY DIFF가 0이 되는데, 이거때문에 문제가 생긴거였다.
```SQL
DATEDIFF(END_DATE, START_DATE)+1 AS DATE_DIFF,
```
를 해주니 잘 풀림

### [7. 조건에 맞는 사용자 정보 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/164670)
```SQL
WITH ID_WITH_OVER_THREE_BOARD AS(
SELECT WRITER_ID
FROM USED_GOODS_BOARD 
WHERE COUNT(WRITER_ID) > 3)
```
로 3개 이상 게시글을 쓴 아이디만 추출하려 했는데 이상하게 에러가 뜬다. GPT에게 물어보니, 
SQL에서 `WHERE` 절 내에서 집계 함수(`COUNT`, `SUM`, `AVG` 등)를 직접 사용하는 것은 허용되지 않습니다. 대신 `HAVING` 절을 사용하여 그룹화된 결과에 대한 조건을 지정해야 합니다. `HAVING` 절은 `GROUP BY` 절과 함께 사용되며, 특정 그룹의 결과가 조건을 만족하는지를 평가합니다.
라고 한다. 그래서 HAVING으로 고쳐줬다.
```SQL
WITH ID_WITH_OVER_THREE_BOARD AS(SELECT WRITER_ID
                                FROM USED_GOODS_BOARD 
                                GROUP BY WRITER_ID
                                HAVING COUNT(WRITER_ID) > 3)
```
010XXXXXXXX를 010-XXXX-XXXX로 나누라는데 이건 어떻게 해야하는지 모르겠어서 찾아봤다.
SUBSTRING으로 나누고 CONCAT으로 붙여주면 되는듯 하다./
```SQL
-- 코드를 입력하세요
WITH ID_WITH_OVER_THREE_BOARD AS(SELECT WRITER_ID AS USER_ID
                                FROM USED_GOODS_BOARD 
                                GROUP BY WRITER_ID
                                HAVING COUNT(WRITER_ID) >= 3)
SELECT USER_ID,
        NICKNAME,
        CONCAT_WS(' ', CITY, STREET_ADDRESS1, STREET_ADDRESS2)AS 전체주소,
        CONCAT_WS('-', SUBSTRING(TLNO,1,3), SUBSTRING(TLNO,4,4), SUBSTRING(TLNO,8,4)) AS 전화번호
FROM USED_GOODS_USER 
WHERE USER_ID IN(SELECT USER_ID
                 FROM ID_WITH_OVER_THREE_BOARD)
ORDER BY USER_ID DESC;
```
그래서 이렇게 했다.



# 번외: WITH 내부에서 LIMIT을 사용하고 싶을때
```SQL
WITH RankedItems AS (
    SELECT 
        ItemID,
        ItemName,
        Price,
        RANK() OVER (ORDER BY Price DESC) AS PriceRank
    FROM Items
)
SELECT * FROM RankedItems
WHERE PriceRank <= 10; -- 이것은 LIMIT와 유사한 효과를 내지만, 순위 기반입니다.
```
	RANK() OVER를 사용하면 된다