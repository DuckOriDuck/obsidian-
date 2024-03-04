### [1. 경기도에 위치한 식품창고 목록 출력하기](https://school.programmers.co.kr/learn/courses/30/lessons/131114) 
```SQL
-- 코드를 입력하세요
SELECT WAREHOUSE_ID, WAREHOUSE_NAME, ADDRESS, CASE FREEZER_YN WHEN IS NULL THEN 'N'
FROM FOOD_WAREHOUSE 
WHERE ADDRESS LIKE '경기도 %'
```
에러가 난다. 그냥 찾아봤다.
```SQL
-- 코드를 입력하세요
SELECT WAREHOUSE_ID, WAREHOUSE_NAME, ADDRESS,IF(FREEZER_YN IS NULL, 'N', FREEZER_YN) AS FREEZER_YN
FROM FOOD_WAREHOUSE 
WHERE ADDRESS LIKE '경기도%'
```
IF()를 써야하나보다.
CASE는 어떻게 쓰는지 찾아봐야겠다
### [7.업그레이드 할 수 없는 아이템 구하기](https://school.programmers.co.kr/learn/courses/30/lessons/273712)
어떻게 해야지 
PARENT_ITEM_ID에 있는 ITEM_ID으 ㅣ값을 추출하나 했는데
EXISTS()를 쓰면 되는것 같다.

```SQL
-- 코드를 작성해주세요
SELECT A.ITEM_ID, A.ITEM_NAME, A.RARITY
FROM ITEM_INFO  A
WHERE NOT EXISTS(SELECT ITEM_ID
                 FROM ITEM_TREE B
                 WHERE A.ITEM_ID = B.PARENT_ITEM_ID)
ORDER BY A.ITEM_ID DESC;
```