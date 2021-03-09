# 하나의 값 조작하기
## 데이터를 가공해야하는 이유
1. 다룰 데이터가 분석용도로 저장되지 않은 경우
2. 로그 데이터, 업무 데이터를 함께 다룰 때, 데이터 형식이 불일치하는 경우

--------

1. 코드 값을 레이블로 변경하기
```
SELECT
  user_id
  , CASE
    WHEN device_type = 1 THEN 'iOS'
    WHEN device_type = 2 THEN 'AND'
    ELSE ''
   END as device_name
```
2. URL에서 요소 추출하기
```
SELECT url_extract_host(td_url), url_extract_path(td_url), url_extract_query(td_url) FROM web_clicks GROUP BY 1,2,3 LIMIT 10 ; 
```
- 참고 : https://prestodb.io/docs/current/functions/url.html
3. 문자열을 배열로 분해하기

7. 날짜와 타임스탬프 다루기
8. 결손 값을 디폴트 값으로 대치하기
9. 
