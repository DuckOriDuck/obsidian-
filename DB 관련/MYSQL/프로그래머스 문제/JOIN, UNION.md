
### [1. 주문량이 많은 아이스크림들 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/133027)
다른 테이블에 있는 TOTAL_ORDER 값을 어떻게 더해야할지 감이 오지 않았다
근데 보니깐
```SQL
-- 코드를 입력하세요
SELECT A.FLAVOR
FROM 
FIRST_HALF A
LEFT JOIN(
SELECT FLAVOR, SUM(TOTAL_ORDER) AS JULY_TOTAL_ORDER
    FROM JULY
    GROUP BY FLAVOR
) B ON A.FLAVOR = B.FLAVOR
ORDER BY(A.TOTAL_ORDER+B.JULY_TOTAL_ORDER) DESC
LIMIT 3
```
이렇게 쓰면 되더라. 어떻게 한거냐면, JULY TABLE의 TOTAL_ORDER를 새로운 COLUMN으로 만들고 `FIRST_HALF` TABLE에 LEFT JOIN 시킨 다음, 서로 다른 행들을 더하게 한 것이다.

### [2. 특정 기간동안 대여 가능한 자동차들의 대여비용 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/157339)
```SQL
-- 코드를 입력하세요 
SELECT A.CAR_ID,
	A.CAR_TYPE, 
	ROUND((A.DAILY_FEE*30)*(1-(B.DISCOUNT_RATE/100))) AS FEE 
FROM 
	CAR_RENTAL_COMPANY_CAR A LEFT 
JOIN (SELECT CAR_TYPE, DISCOUNT_RATE 
	  FROM CAR_RENTAL_COMPANY_DISCOUNT_PLAN 
	  WHERE DURATION_TYPE = '30일 이상') B ON A.CAR_TYPE= B.CAR_TYPE 
WHERE A.CAR_TYPE IN ('세단, SUV') AND 
	CAR_ID IN(SELECT CAR_ID 
	FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY 
	WHERE START_DATE>'2022-11-30' OR END_DATE<'2022-11-01') AND 
	FEE>=500000 AND FEE<=2000000 
ORDER BY 
		FEE DESC, A.CAR_TYPE ASC, A.CAR_ID ASC 
```
이렇게 쓰니깐`SQL 실행 중 오류가 발생하였습니다. Unknown column 'FEE' in 'IN/ALL/ANY subquery'`라는데 뭐가 문제일까.  GPT에게 물어보니, 
```SQL 
"Unknown column 'FEE' in 'IN/ALL/ANY subquery'"는 `FEE`라는 열이 WHERE 절에서 인식되지 않았다는 것을 의미합니다.
`FEE`가 실제 테이블의 열이 아니라, SELECT 문에서 생성된 별칭(alias)이기 때문입니다. SQL의 WHERE 절에서는 SELECT 절에 정의된 별칭을 직접 사용할 수 없습니다. 대신, HAVING 절을 사용하거나, 필터링 조건을 외부 쿼리로 이동해야 합니다.
```
라고 해서 
```SQL
-- 코드를 입력하세요
SELECT A.CAR_ID, A.CAR_TYPE, ROUND((A.DAILY_FEE*30)*(1-(B.DISCOUNT_RATE/100))) AS FEE
FROM 
CAR_RENTAL_COMPANY_CAR A
LEFT JOIN (SELECT CAR_TYPE, DISCOUNT_RATE
          FROM CAR_RENTAL_COMPANY_DISCOUNT_PLAN 
          WHERE DURATION_TYPE = '30일 이상') B ON A.CAR_TYPE= B.CAR_TYPE
          
WHERE A.CAR_TYPE IN ('세단, SUV')
AND A.CAR_ID NOT IN(SELECT CAR_ID
               FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY
               WHERE START_DATE<='2022-11-30' AND END_DATE >= '2022-11-01')
HAVING FEE>=500000 AND FEE<2000000
ORDER BY FEE DESC, A.CAR_TYPE ASC, A.CAR_ID ASC
```
로 풀었는데 제대로 나오지 않는다. 이번엔 무엇이 문제였던 걸까
```SQL
WHERE A.CAR_TYPE IN ('세단', 'SUV')
```
부분에서 이상하게 써서 그랬던 것이다. 이 부분만 고치면 어떻게든 된다.
아래는 서브쿼리 없이 더 간결하게 쓰기 위해 적은 것.

```SQL
SELECT A.CAR_ID, A.CAR_TYPE, ROUND((A.DAILY_FEE*30)*(1-(B.DISCOUNT_RATE/100))) AS FEE
FROM 
CAR_RENTAL_COMPANY_CAR A
LEFT JOIN 
CAR_RENTAL_COMPANY_DISCOUNT_PLAN B
ON DURATION_TYPE = '30일 이상' AND A.CAR_TYPE = B.CAR_TYPE
          
WHERE A.CAR_TYPE IN ('세단', 'SUV')
AND A.CAR_ID NOT IN(SELECT CAR_ID
               FROM CAR_RENTAL_COMPANY_RENTAL_HISTORY C
               WHERE C.START_DATE<='2022-11-30' AND C.END_DATE >= '2022-11-01')
HAVING FEE>=500000 AND FEE<2000000
ORDER BY FEE DESC, A.CAR_TYPE ASC, A.CAR_ID ASC
```

### [3. 5월 식품들의 총매출 조회하기](https://school.programmers.co.kr/learn/courses/30/lessons/131117)
```SQL
-- 코드를 입력하세요
SELECT A.PRODUCT_ID, A.PRODUCT_NAME, (A.PRICE*B.AMOUNT) AS TOTAL_SALES
FROM FOOD_PRODUCT A
LEFT JOIN 
FOOD_ORDER B ON A.PRODUCT_ID = B.PRODUCT_ID
WHERE PRODUCE_DATE LIKE '2022-05%'
ORDER BY TOTAL_SALES DESC, PRODUCT_ID ASC;
```
오답이었다. 알고 보니 FOOD_ORDER에 PRODUCT_ID가 중복되는 것들이 많았던 것.
그래서
```SQL
-- 코드를 입력하세요
SELECT A.PRODUCT_ID, A.PRODUCT_NAME, SUM(A.PRICE*B.AMOUNT) AS TOTAL_SALES
FROM FOOD_PRODUCT A
LEFT JOIN 
FOOD_ORDER B ON A.PRODUCT_ID = B.PRODUCT_ID
WHERE PRODUCE_DATE LIKE '2022-05%'
GROUP BY PRODUCT_ID
ORDER BY TOTAL_SALES DESC, PRODUCT_ID ASC;
```
이렇게 고쳐주었따.
문제를 제대로 보자.

### [5. 1. 그룹별 조건에 맞는 식당 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131124)
```SQL
-- 코드를 입력하세요
SELECT MP.MEMBER_NAME, RR.REVIEW_TEXT, RR.REVIEW_DATE
FROM REST_REVIEW RR
LEFT JOIN MEMBER_PROFILE MP 
ON RR.MEMBER_ID = MP.MEMBER_ID
ORDER BY COUNT(RR.MEMBER_ID) DESC
LIMIT 1;
```
했는데.
```SQL
SQL 실행 중 오류가 발생하였습니다.  
Expression #1 of ORDER BY contains aggregate function and applies to the result of a non-aggregated query
```
라고 뜸.

GPT에게 물어보니, CTE를 쓰는것을 추천해줬다.
```
CTE가 뭐야?
ChatGPT

CTE는 Common Table Expression의 약자로, SQL에서 사용되는 일시적인 결과 집합을 의미합니다. CTE는 복잡한 쿼리를 더 읽기 쉽고 유지 관리하기 쉬운 여러 논리적 단계로 분할하는 데 유용합니다. 기본적으로, CTE는 쿼리 내에서 임시 결과 집합을 생성하고, 이 결과 집합은 쿼리가 실행되는 동안만 존재하며, 쿼리 실행이 완료되면 사라집니다.

CTE는 `WITH` 절을 사용하여 정의됩니다. 이 절 다음에는 CTE의 이름과, 괄호 안에 정의된 열(선택 사항), 그리고 CTE의 본문이 오는데, 이는 일반적으로 SELECT 문입니다. CTE는 복잡한 JOIN, 서브쿼리, 재귀 쿼리 등을 더 명확하게 표현할 수 있게 해 줍니다.

예를 들어, 어떤 테이블에서 데이터를 요약하고, 그 요약된 결과를 바탕으로 추가적인 쿼리를 수행하고 싶은 경우, CTE를 사용하여 이를 구현할 수 있습니다:
```
CTE를 통해서 임시 테이블을 만들고 그걸 이용해서 JOIN을 한다
```SQL
-- 코드를 입력하세요
WITH REVIEW_COUNT AS (
    SELECT MEMBER_ID, COUNT(*) AS COUNT
    FROM REST_REVIEW
    GROUP BY MEMBER_ID
    ORDER BY COUNT DESC
    LIMIT 1
)
    
SELECT MP.MEMBER_NAME, RR.REVIEW_TEXT, DATE_FORMAT(RR.REVIEW_DATE, '%Y-%m-%d') AS REVIEW_DATE
FROM REST_REVIEW RR
JOIN MEMBER_PROFILE MP ON RR.MEMBER_ID = MP.MEMBER_ID
JOIN REVIEW_COUNT RC ON RR.MEMBER_ID = RC.MEMBER_ID
ORDER BY RR.REVIEW_DATE, RR.REVIEW_TEXT
```


### [11.1. 상품을 구매한 회원 비율 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/131534)
1트: 불합격이 떴다. 
그래서 만든 2트:
```SQL
WITH USER_INFO_SUB AS (
    SELECT USER_ID
    FROM USER_INFO
    WHERE JOINED LIKE '2021-%' 
                      )
                      
WITH JOINED_ONLINE_SALES AS( 
    SELECT YEAR(SALES_DATE) AS YEAR,
    MONTH(SALES_DATE) AS MONTH,
    COUNT(A.USER_ID) AS PUCHASED_USERS,
    FROM ONLINE_SALE
    JOIN USER_INFO_SUB B ON A.USER_ID = B.USER_ID
)

SELECT YEAR,
	MONTH,
	 COUNT(USER_ID) AS PUCHASED_USERS,
	  ROUND(COUNT(USER_ID) / (SELECT COUNT(*) FROM USER_INFO_SUB),1) AS PUCHASED_RATIO
FROM JOINED_ONLINE_SALES 
GROUP BY YEAR, MONTH
ORDER BY YEAR, MONTH;

```
```
SQL 실행 중 오류가 발생하였습니다.  
You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'FROM ONLINE_SALE  
JOIN USER_INFO_SUB B ON A.USER_ID = B.USER_ID  
)  
  
SELECT YEA' at line 13
```
에러가 난다. GPT에게 물어보았다.

![User](https://lh3.googleusercontent.com/a/AGNmyxbeE1ZM16wJwQU_kXX6kB4GAKbsVgpNtFKe7029bQ=s96-c)

당신

왜 대답을 못해

ChatGPT

ChatGPT

죄송합니다, 당신의 문제를 해결하기 위해 여기에 몇 가지 수정 사항을 제안하겠습니다. SQL 쿼리에서 발생한 오류를 수정하기 위해 다음 사항들을 고려해야 합니다:

1. 여러 개의 CTE(Common Table Expressions)를 사용할 때, `WITH` 구문은 첫 번째 CTE에서만 사용하고, 이후의 CTE들은 쉼표(,)로 구분해야 합니다.
    
2. `COUNT(A.USER_ID)` 구문에서 `A`라는 테이블 별칭이 정의되지 않았습니다. 올바른 테이블 별칭을 사용해야 합니다.
    
3. `PUCHASED_RATIO`를 계산할 때, 분모는 `2021년에 가입한 전체 회원 수`이므로, 이를 올바르게 계산해야 합니다.
    
4. `ROUND` 함수에서 소수점 두 번째 자리에서 반올림하려면, 두 번째 인자로 2를 넣어야 합니다.
말 잘 듣고 수정했다.
```SQL
-- 코드를 입력하세요
-- 2021년에 가입한 회원
WITH USER_INFO_SUB AS (
    SELECT USER_ID
    FROM USER_INFO
    WHERE JOINED LIKE '2021-%' 
), JOINED_ONLINE_SALES AS( 
    SELECT 
        YEAR(SALES_DATE) AS YEAR,
        MONTH(SALES_DATE) AS MONTH,
        USER_ID
    FROM ONLINE_SALE 
    WHERE USER_ID IN(SELECT USER_ID FROM USER_INFO_SUB)
)

SELECT 
    YEAR,
    MONTH,
    COUNT(DISTINCT USER_ID) AS PUCHASED_USERS,
    ROUND(COUNT(DISTINCT USER_ID) / (SELECT COUNT(*) FROM USER_INFO_SUB),1) AS PUCHASED_RATIO
FROM JOINED_ONLINE_SALES 
GROUP BY YEAR, MONTH
ORDER BY YEAR, MONTH;

```
알고보니, `ONLINE_SALE`에서 유저가 겹칠 가능성이 높아서 DISTINCT를 써야 하는거시였다.