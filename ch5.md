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
```
select url_extract_path(td_url), SPLIT(url_extract_path(td_url), '/')[1] AS PATH1, SPLIT(url_extract_path(td_url), '/')[2] AS PATH2, SPLIT(url_extract_path(td_url), '/')[3] AS PATH2 from web_clicks group by 1,2,3 limit 10 ; 
```
4. 날짜와 타임스탬프 다루기
```
select current_date, current_time, current_timestamp, localtime, localtimestamp, current_timezone() ;
```
- 문자열을 날짜로 convert
```
select date('2021-03-09') as date, '2021-03-09' as str;
```
- 특정 필드 추출
```
SELECT timestamp '2012-10-31 01:35:00 UTC' AT TIME ZONE 'Asia/Seoul', hour(timestamp '2012-10-31 01:35:00 UTC' AT TIME ZONE 'Asia/Seoul');
```

5. 결손 값을 디폴트 값으로 대치하기

