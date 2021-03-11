#여러 개의 값에 대한 조작

## 새로운 지표 정의하기
- 페이지뷰  / 방문자수 -> 사용자 한 명이 페이지를 몇번이나 방문했는가? 
- CTR(클릭 비율), CVR(컨버전 비율) 등 지표 정의

--------

1. 문자열 연결하기
```
SELECT CONCAT(valu1, value2);
```
2. 여러개의 값 비교하기
```
SELECT 
 CASE
  WHEN q1 < q2 THEN '+'
  WHEN q1 = q2 THEN ''
  ELSE '-'
 END AS judge_q1_q2,
 SIGN(q2 - q1) AS sign_q2_q1;

greatest(q1, q2, q3, q4)

least(q1, q2, q3, q4)
```
3. 2개의 값 비율 계산하기
```
-- CTR 계산
SELECT 100.0 * clicks / impressions;
```
- 분모가 0인 경우, NULL로 치환할 것
6. 두 값의 거리 계산하기
```
abs(x1 - x2) as 절대값
sqrt(power(x1 - x2, 2)) as 제곱 평균 제곱근

```
8. 날짜/시간 계산하기
9. IP 주소 다루기

