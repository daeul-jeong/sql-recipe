# 하나의 테이블에 대한 조작
## 데이터 집약
- 집약 함수 : 여러개의 레코드를 기반으로 하나의 값을 리턴하는 함수
- ex ) sum, count
## 데이터 가공

1. 그룹의 특징 잡기
```
SELECT
 user_id,
 count(*) as total_count,
 count(distince product_id),
 sum(score),
 max(score),
 min(score)
from
 review
 group by user_id
```
- group by 구문에서는 지정한 컬럼을 유니크 키로 새로운 테이블이 만들어짐

```
select
 user_id,
 score,
 avg(score) over() as 전체 평균 리뷰 점수,
 avg(score) over(partition by user_id) as 사용자 평균 리뷰 점수
```
- 집약함수(sum, count, avg.. 등등) 뒤에 OVER()를 붙이면 테이블 전체 적용
- OVER(partition by 컬럼명) 하면 컬럼 기준으로 그룹화하여 집약함수를 적용

```
select
 product_id,
 score,
 ROW_NUMBER() OVER(ORDER BY score DESC) as row,
 RANK() OVER(ORDER BY score DESC) as rank,
 LAG(product_id) OVER(ORDER BY score DESC), -- score 순으로 정렬 후 앞에 있는 값 조회
 LEAD(product_id) OVER(ORDER BY score DESC) -- score 순으로 정렬 후 뒤에 있는 값 조회
```

```
select 
-- 순위 상위부터의 누계 점수 계산
 SUM(score) OVER(ORDER BY score DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS cum_score ,
 -- 현재 행과 앞 뒤의 행이 가진 값으로 평균 구하기
 AVG(score) OVER(ORDER BY score DESC ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING),
 -- 순위가 높은 상품 ID 추출
 FIRST_VALUE(product_id) OVER(ORDER BY score DESC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING),
 -- 순위가 낮은 상품 ID 추출
 LAST_VALUE(product_id) OVER(ORDER BY SCORE DESC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING)
 
```

- 프레임 지정 구문
 - `ROWS BETWEEN start AND end`
  - start와 end에 들어갈 수 있는 값
   - CURRENT ROW(현재의 행)
   - n PRECEDING(n행 앞)
   - n FOLLOWING(n행 뒤)
   - UNBOUNDED PRECEDING(이전 행 전부)
   - UNBOUNDED FOLLOWING(이후 행 전부) 

```
select 
-- 가장 앞 순위부터 가장 뒷 순위까지 범위를 대상으로 상품 ID 집약
 array_agg(product_id) OVER(ORDER BY score DESC ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING),
 -- 가장 앞 순위부터 현재 순위까지 범위를 대상으로 상품 ID 집약
 array_agg(product_id) OVER(ORDER BY SCREO DESC ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW),
 -- 순위 하나 앞과 하나 뒤까지의 범위를 대상으로 상품 ID 집약
 array_agg(product_id) OVER(ORDER BY SCORE DESC ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING)
 
```

- PARTITION BY 와 ORDER BY 조합하기

```
select 
 category,
 product_id,
 score,
 -- 카테고리별로 점수 순서로 정렬하고 유일한 순위를 붙임
 ROW_NUMBER() OVER(PARTITION BY category ORDER BY score DESC) as row
```

- 각 카테고리의 상위 n개 추출하기
```
select * from 
-- 서브쿼리로 순위 계산
 (
  select 
   category,
   product_id,
   score,
   row_number() over(partition by category order by score desc) as rank from products
 ) as rank_products
 where rank <-2
```


3. 세로 기반 데이터를 가로 기반으로 변환하기
dt    | indicator   | value
01-01 | impressions | 1800
01-01 | sessions   | 50
01-01 | users       | 200
01-02 | impressions | 1800
01-02 | sessions   | 50
01-02 | users       | 200

```
select
 dt,
 max(case when indicator = 'impressions' then value end) as impressions,
 max(case when indicator = 'sessions' then value end) as sessions,
 max(case when indicator = 'users' then value end) as users
```

```
-- 상품 ID를 배열에 집약하고 쉼표로 구분된 문자열로 변환하기
select 
 purhcase_id,
 string_agg(product_id, ',') as products_ids
```
> A001, A002, A003 이렇게 쉼표로 한칸에 나옴.














